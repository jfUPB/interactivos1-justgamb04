### Análisis del código

#### Setup inicial

```js
const express = require('express');
const http = require('http');
const socketIO = require('socket.io');

const app = express();
const server = http.createServer(app);
const io = socketIO(server);
const port = 3000;
```

En esta parte, se importan las dependencias necesarias:

* **express**: facilita la creación del servidor y la gestión de rutas.
* **http**: permite crear un servidor HTTP que servirá como base.
* **socket.io**: habilita la comunicación en tiempo real mediante WebSockets.

Se crea una instancia de Express, se usa para crear un servidor HTTP, y luego se vincula Socket.IO a ese servidor. Finalmente, se define que se escuche localmente en el puerto 3000 (que será expuesto al exterior mediante Dev Tunnels).

#### Servir archivos cliente

```js
app.use(express.static('public'));
```

Este comando configura el servidor para que sirva automáticamente cualquier archivo dentro de la carpeta `public`. Esto simplifica la gestión de rutas, ya que no es necesario definirlas manualmente como con `app.get('/ruta', ...)`, utilizado en la Unidad 6. Por ejemplo, una petición a `/desktop/` devolverá automáticamente `public/desktop/index.html`.

#### Manejo de conexiones y mensajes

```js
io.on('connection', (socket) => {
    console.log('New client connected - ID:', socket.id);

    socket.on('message', (message) => {
        console.log(`Received message => ${message} from ID: ${socket.id}`);
        socket.broadcast.emit('message', message);
    });

    socket.on('disconnect', () => {
        console.log('Client disconnected - ID:', socket.id);
    });
});
```

* `io.on('connection', ...)`: se activa cuando un nuevo cliente se conecta.
* Cada cliente conectado recibe un identificador único (`socket.id`).
* `socket.on('message', ...)`: escucha cuando ese cliente envía un mensaje (en este caso, desde el móvil).
* `socket.broadcast.emit('message', message)`: retransmite ese mensaje a todos los demás clientes, **excepto** al que lo envió.
* `socket.on('disconnect', ...)`: registra cuando un cliente se desconecta.

#### Inicio del servidor

```js
server.listen(port, () => {
    console.log(`Server is listening on http://localhost:${port}`);
});
```

Esta línea pone en marcha el servidor para escuchar conexiones en el puerto definido. El mensaje de consola confirma que el servidor está funcionando. Dev Tunnels se encargará de que esta dirección local sea accesible desde otros dispositivos.

### Desarrollo preguntas de reflexión

#### ¿Cuál es la función principal de `express.static('public')` y cómo se compara con `app.get()`?

La función principal de `express.static('public')` es servir automáticamente cualquier archivo estático (HTML, CSS, JS, imágenes) ubicado dentro de la carpeta `public`. Es más eficiente que usar `app.get()` para cada archivo o ruta, ya que elimina la necesidad de declarar manualmente cada una. Esta automatización simplifica el código y es más escalable para aplicaciones con muchas vistas o recursos.

#### Explicación del flujo de un mensaje táctil:

1. El evento `touchMoved()` en el cliente móvil detecta movimiento del dedo.
2. El móvil usa `socket.emit('message', mensaje)` para enviar un objeto en formato JSON con las coordenadas del toque.
3. El servidor escucha este mensaje con `socket.on('message', ...)`.
4. Luego, retransmite el mensaje a todos los demás clientes mediante `socket.broadcast.emit('message', mensaje)`.
5. El cliente de escritorio recibe el mensaje con su propio `socket.on('message', ...)` y lo procesa para actualizar la visualización.

Se usa `socket.broadcast.emit` para evitar que el emisor (el móvil) reciba su propio mensaje. `io.emit` lo enviaría a todos, incluido el emisor; `socket.emit` solo lo enviaría al cliente que lo originó.

#### Si conectaras dos computadores de escritorio y un móvil al servidor, ¿quién recibiría el mensaje si el móvil envía datos táctiles?

Ambos computadores de escritorio recibirían el mensaje. El móvil no lo recibiría porque el servidor usa `socket.broadcast.emit`, que explícitamente **excluye al emisor**. Así, cualquier mensaje enviado por el móvil será retransmitido a todos los demás clientes conectados (en este caso, los escritorios).

#### ¿Qué información útil te proporcionan los `console.log` en el servidor?

* Notificaciones de nuevas conexiones y desconexiones de clientes, junto con sus identificadores únicos.
* Registro de los mensajes que llegan al servidor, lo que permite verificar que los datos se están transmitiendo correctamente.
* Identificación del origen de cada mensaje, útil para depuración y para entender el comportamiento de los diferentes clientes conectados.
