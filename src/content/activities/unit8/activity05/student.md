**Inputs y el puente Node.js**
**Fecha:** 11 de mayo de 2025

## Estructura del Proyecto

```
my-project/
│
├── public/
│   ├── index.html
│   ├── desktop/
│   │   └── sketch.js
│   └── mobile/
│       └── sketch.js
│
├── server.js
└── package.json
```
## Fragmentos Clave del Código

### Micro\:bit (MicroPython)

```python
from microbit import *
import random

while True:
    x = accelerometer.get_x()
    y = accelerometer.get_y()
    z = accelerometer.get_z()
    print("X:{} Y:{} Z:{}".format(x, y, z))
    sleep(200)
```

### Cliente Móvil (public/mobile/sketch.js)

```javascript
let socket;

function setup() {
  createCanvas(windowWidth, windowHeight);
  socket = io();

  // Envío al tocar
  touchStarted = () => {
    socket.emit('mobileInput', {
      touchX: mouseX,
      touchY: mouseY
    });
  };
}
```

### Servidor Puente (server.js)

```javascript
const express = require('express');
const app = express();
const http = require('http').createServer(app);
const io = require('socket.io')(http);

app.use(express.static('public'));

io.on('connection', (socket) => {
  console.log('Cliente conectado');

  socket.on('mobileInput', (data) => {
    io.emit('mobileInputForwarded', data); // reenvío directo
  });
});

http.listen(3000, () => {
  console.log('Servidor corriendo en puerto 3000');
});
```

### Cliente p5.js (public/desktop/sketch.js)

```javascript
let socket;
let serial;
let latestData = "";

function setup() {
  createCanvas(800, 600);
  serial = new p5.SerialPort();

  // Serial
  serial.open('/dev/tty.usbmodemXXXX'); // cambiar por puerto correspondiente
  serial.on('data', () => {
    let currentString = serial.readLine();
    if (currentString) {
      latestData = currentString.trim();
      console.log("Serial: ", latestData);
    }
  });

  // Socket
  socket = io();
  socket.on('mobileInputForwarded', (data) => {
    console.log("Socket: ", data);
  });
}
```
## Evidencia de Flujo de Datos

### Consola p5.js (navegador):

```
Serial:  X:23 Y:-4 Z:1011
Serial:  X:25 Y:-6 Z:1009
Socket:  {touchX: 317, touchY: 562}
Socket:  {touchX: 298, touchY: 540}
```

## Desafíos Encontrados

* **Conexión Web Serial**: identificar el puerto correcto fue clave; en algunos navegadores fue necesario recargar y volver a solicitar permisos.
* **Formato del dato serial**: al principio, el micro\:bit enviaba el string sin salto de línea, lo que generaba concatenación de datos.
* **Sockets duplicados**: al recargar el cliente móvil sin cerrar bien el socket, se enviaban múltiples copias del mismo evento.

## Verificación Exitosa

✔ Micro\:bit envía datos por Serial y son leídos correctamente en p5.js.
✔ Cliente móvil envía datos por Socket.IO.
✔ Servidor puente reenvía correctamente sin modificar los datos.
✔ Cliente p5.js recibe e imprime los datos crudos por consola.
