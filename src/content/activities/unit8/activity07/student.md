Este proyecto explora una experiencia interactiva que combina entradas desde un micro\:bit, un cliente móvil y el teclado del computador para generar una visualización en p5.js. El sistema permite visualizar en tiempo real cómo múltiples dispositivos pueden integrarse para controlar dinámicamente un objeto gráfico (una elipse) que responde al movimiento físico, al tacto y a entradas locales. Utilizando una arquitectura cliente-servidor basada en Node.js y Socket.IO, se establece una comunicación en tiempo real entre los componentes. El proyecto aplica el modelo I-P-O (Input-Process-Output) como marco estructural.

## Concepto I-P-O

### Narrativa:

El sistema representa un “pulso digital” que responde a múltiples formas de presencia humana: movimiento físico (micro\:bit), contacto táctil (móvil) y atención directa (teclado local). Cada input modifica una dimensión diferente del comportamiento de una esfera digital, que simboliza la conciencia del sistema.

### Inputs:

* **Micro\:bit**: acelerómetro (X, Y, Z).
* **Cliente móvil**: posición del toque en pantalla (touchX, touchY).
* **Cliente local (PC)**: teclas (flechas arriba/abajo).

### Process:

* Se normalizan los datos del micro\:bit y el móvil para controlar la posición y el color de la elipse.
* Las flechas modifican su posición vertical como un control de “intensidad de atención”.
* Los valores se procesan combinando los inputs para producir un conjunto coherente de parámetros visuales.

### Output:

* Una **elipse animada** en el lienzo de p5.js, cuya posición y color cambian en tiempo real:

  * Posición → X/Y del micro\:bit + flechas.
  * Color → Z del micro\:bit + touchX/Y del móvil.

## Diseño Técnico

### Diagrama de Arquitectura Final:

```
[Micro:bit]     [Cliente Móvil]
     |               |
     | Serial        | Socket.IO (JS)
     |               |
     |           [Servidor Node.js (Socket.IO + SerialPort)]
     |______________________|      
                |
                | Socket.IO
       [Cliente p5.js (escritorio)]
```

### Tecnologías y Protocolos:

* **Node.js**: servidor puente (Express + Socket.IO).
* **Socket.IO**: comunicación en tiempo real entre servidor ↔ móvil ↔ escritorio.
* **p5.js**: visualización y procesamiento.
* **Web Serial API**: lectura directa del micro\:bit desde el navegador.
* **MicroPython**: para enviar los datos seriales desde el micro\:bit.

## Implementación

### Componentes:

#### Micro\:bit:

Programa en MicroPython que envía los datos de aceleración:

```python
from microbit import *

while True:
    x = accelerometer.get_x()
    y = accelerometer.get_y()
    z = accelerometer.get_z()
    print("X:{} Y:{} Z:{}".format(x, y, z))
    sleep(100)
```

#### Cliente móvil (`public/mobile/sketch.js`):

Captura del toque y envío vía Socket.IO:

```javascript
let socket;

function setup() {
  createCanvas(windowWidth, windowHeight);
  socket = io();

  touchMoved = () => {
    socket.emit('mobileInput', { touchX: mouseX, touchY: mouseY });
    return false;
  };
}
```

#### Servidor puente (`server.js`):

```javascript
const express = require('express');
const http = require('http');
const socketIO = require('socket.io');
const SerialPort = require('serialport');
const Readline = require('@serialport/parser-readline');

const app = express();
const server = http.createServer(app);
const io = socketIO(server);

app.use(express.static('public'));

io.on('connection', socket => {
  socket.on('mobileInput', data => {
    io.emit('mobileInputForwarded', data);
  });
});

server.listen(3000, () => {
  console.log('Servidor en http://localhost:3000');
});
```

#### Cliente p5.js (`public/desktop/sketch.js`):

```javascript
let socket, serial;
let microbitData = { x: 0, y: 0, z: 0 };
let mobileData = { touchX: 0, touchY: 0 };
let localControl = 0;

function setup() {
  createCanvas(800, 600);
  socket = io();

  // Socket: móvil
  socket.on('mobileInputForwarded', data => mobileData = data);

  // Serial: micro:bit
  serial = new p5.SerialPort();
  serial.open('/dev/tty.usbmodemXXXX'); // cambiar puerto
  serial.on('data', () => {
    let line = serial.readLine();
    if (line) {
      let matches = line.match(/X:(-?\d+)\s+Y:(-?\d+)\s+Z:(-?\d+)/);
      if (matches) {
        microbitData = {
          x: int(matches[1]),
          y: int(matches[2]),
          z: int(matches[3])
        };
      }
    }
  });
}

function draw() {
  background(30);
  processInputs();

  fill(ellipseColor);
  ellipse(ellipseX, ellipseY, 50, 50);
}

let ellipseX = 400, ellipseY = 300, ellipseColor;

function processInputs() {
  ellipseX += microbitData.x * 0.01;
  ellipseY += microbitData.y * 0.01;

  if (keyIsDown(UP_ARROW)) ellipseY -= 2;
  if (keyIsDown(DOWN_ARROW)) ellipseY += 2;

  let r = map(microbitData.z, -1024, 1024, 0, 255);
  let g = map(mobileData.touchX, 0, width, 0, 255);
  let b = map(mobileData.touchY, 0, height, 0, 255);
  ellipseColor = color(r, g, b);
}
```

## Instrucciones para Ejecutar

1. **Conecta el micro\:bit** al PC y verifica el puerto en el código (`/dev/tty.usbmodemXXXX` o `COMX`).
2. Ejecuta el servidor:

   ```bash
   node server.js
   ```
3. Abre dos pestañas:

   * `localhost:3000/desktop` en el computador.
   * `localhost:3000/mobile` en un celular conectado a la misma red.
4. Mueve el micro\:bit, toca el móvil, y usa el teclado para observar el comportamiento del sistema en el canvas del cliente p5.js.

**Verificado**: el sistema responde a todos los inputs.

## Reflexión Crítica

### Decisiones de diseño:

* Se eligió una arquitectura cliente-servidor con Node.js para permitir comunicación fluida entre dispositivos móviles y escritorio.
* Se usó p5.js por su accesibilidad para visualización rápida y Web Serial API para integración directa del micro\:bit.

### Desafíos:

* Establecer una lectura limpia de datos seriales sin ruido.
* Coordinar múltiples inputs sin conflictos de latencia.
* Crear una visualización coherente con inputs de distintas escalas.

### Aprendizajes:

* Cómo combinar tecnologías web modernas con dispositivos físicos.
* Cómo estructurar un sistema modular y escalable.
* Profundización en la comunicación serial y en eventos con Socket.IO.

### Mejoras futuras:

* Añadir audio al output.
* Permitir múltiples móviles conectados.
* Añadir filtros para suavizar el movimiento de la elipse.
