### Proyecto: *Lienzo Caótico: La danza del orden y el caos*

## Diagrama de Arquitectura (Servidor Puente)

**Componentes:**

![image](https://github.com/user-attachments/assets/836b599c-fdd9-4fd9-b4c8-ac8efad3a520)

> *Dev Tunnels (como `localhost.run`, `ngrok`, etc.) se usarán solo si el móvil no está en la misma red o no puede acceder al host del servidor.*

## Tecnologías de Comunicación

| Flujo de Datos     | Tecnología          | Protocolo                          |
| ------------------ | ------------------- | ---------------------------------- |
| Micro\:bit → p5.js | Web Serial API      | ASCII (Aceleración)                |
| Móvil → Servidor   | Socket.IO (cliente) | JSON (evento touch)                |
| Servidor → p5.js   | Socket.IO (server)  | JSON (reenviado igual)             |
| PC Local → p5.js   | Eventos nativos     | `keyPressed`, `mousePressed`, etc. |

## Protocolos de Datos

### Micro\:bit → Cliente (Serial):

* **Formato ASCII**: ejemplo de línea:

  ```
  "x: -102 y: 305 z: 988"
  ```
* **Parseo esperado**: extraer valores de aceleración `x`, `y`, `z`.

### Móvil → Servidor (Socket.IO):

* **Evento:** `"touch"`
* **Payload JSON:**

  ```json
  {
    "touchX": 182,
    "touchY": 435,
    "intensity": 0.75
  }
  ```

### Servidor → Cliente p5.js (Socket.IO):

* **Evento reenviado:** `"touch"`
* **Payload JSON:** el mismo recibido, sin modificaciones.

## Esquema lógico del Servidor Node.js (MUY SIMPLE)

### Pseudocódigo `server.js`:

```javascript
io.on('connection', (socket) => {
  socket.on('touch', (data) => {
    // Reenviar sin modificar
    io.emit('touch', data);
  });
});
```

* El servidor **no almacena estado**.
* Solo reenvía cada evento `"touch"` recibido desde el móvil al cliente p5.js.

## Esquema lógico del Cliente p5.js (DETALLADO)

### Pseudocódigo del flujo de `sketch.js`:

```javascript
// Variables de estado
let trazoX, trazoY;
let velocidadX = 0, velocidadY = 0;
let caosNivel = 0;
let modoEstilo = "default";

function setup() {
  createCanvas(windowWidth, windowHeight);
  connectToSerial(); // Configura Web Serial con micro:bit
  connectToSocket(); // Configura conexión con el servidor puente
}

function connectToSerial() {
  // Inicia la comunicación serial
  // Parsea líneas ASCII para actualizar velocidadX/Y
}

function connectToSocket() {
  socket = io.connect(SERVER_URL);
  socket.on('touch', (data) => {
    // Aumenta el caos y genera ráfagas
    caosNivel += data.intensity;
    generarRafaga(data.touchX, data.touchY, data.intensity);
  });
}

function keyPressed() {
  if (key === '1') modoEstilo = "abstracto";
  if (key === '2') modoEstilo = "glitch";
  if (key === '3') modoEstilo = "monocromo";
}

function draw() {
  // Actualiza posición del trazo con velocidad
  trazoX += velocidadX;
  trazoY += velocidadY;

  // Aplica estilo según el modo
  aplicarEstilo(modoEstilo, caosNivel);

  // Dibuja el trazo principal
  dibujarTrazo(trazoX, trazoY);

  // Reduce caos con el tiempo
  caosNivel *= 0.95;
}
```

## Output Visual (Resumen técnico)

* La pantalla muestra un lienzo donde:

  * El **movimiento del micro\:bit** guía un pincel visual.
  * Los **toques desde el móvil** causan efectos caóticos (explosiones de color, distorsiones).
  * El usuario puede alternar modos visuales con el **teclado**.
* Cada input afecta variables que controlan el estilo visual de `draw()`.


![image](https://github.com/user-attachments/assets/836b599c-fdd9-4fd9-b4c8-ac8efad3a520)
