### Idea creativa: **“Ping de Reacción”**

Una aplicación web en tiempo real que mide los reflejos de dos usuarios. Uno de ellos (el **emisor**) inicia una señal de "¡clic ya!" desde su pantalla. El otro (el **receptor**) debe reaccionar lo más rápido posible haciendo clic. El tiempo de reacción se mide y se le muestra al emisor.

### Implementación:

#### ¿Qué información se intercambia?

* El emisor envía el "ping".
* El receptor recibe la señal y hace clic.
* El tiempo de reacción es enviado de vuelta al emisor.

#### ¿Cómo se visualiza?

* El emisor tiene un botón para iniciar el "ping".
* El receptor ve una pantalla roja con el texto "¡Haz clic ahora!" y hace clic.
* Luego ambos ven el tiempo de reacción.

## Enlace a la actividad
https://glitch.com/edit/#!/gregarious-light-tuck

##  ESTRUCTURA DE ARCHIVOS Y CÓDIGO

Crea una carpeta con esta estructura:

```
actividad-05/
├── public/
│   ├── page1.html
│   ├── page2.html
│   ├── page1.js
│   └── page2.js
├── server.js
├── package.json
```

### `server.js` 

```js
const express = require('express');
const app = express();
const http = require('http').Server(app);
const io = require('socket.io')(http);

app.use(express.static('public'));

io.on('connection', (socket) => {
    console.log('Usuario conectado:', socket.id);

    socket.on('startPing', () => {
        socket.broadcast.emit('pingNow');
    });

    socket.on('reaction', (reactionTime) => {
        socket.broadcast.emit('result', reactionTime);
    });
});

http.listen(3000, () => {
    console.log('Servidor en http://localhost:3000');
});
```

### `public/page1.js` 

```js
let socket = io('http://localhost:3000');
let lastReaction = null;

function setup() {
    createCanvas(400, 200);
    let btn = createButton("Iniciar Ping");
    btn.position(10, 10);
    btn.mousePressed(() => {
        socket.emit('startPing');
    });
    textAlign(CENTER, CENTER);
    textSize(20);
}

socket.on('result', (reactionTime) => {
    lastReaction = reactionTime;
});

function draw() {
    background(220);
    text("Emisor - Inicia el ping", width / 2, height / 2 - 20);
    if (lastReaction !== null) {
        text("Tiempo de reacción: " + nf(lastReaction, 1, 2) + " ms", width / 2, height / 2 + 30);
    }
}
```

### `public/page2.js`

```js
let socket = io('http://localhost:3000');
let waiting = false;
let pingTime = 0;
let reacted = false;

function setup() {
    createCanvas(400, 200);
    background(200);
    textAlign(CENTER, CENTER);
    textSize(20);
}

socket.on('pingNow', () => {
    waiting = true;
    pingTime = millis();
    reacted = false;
    background(255, 0, 0);
    text("¡Haz clic ahora!", width / 2, height / 2);
});

function mousePressed() {
    if (waiting && !reacted) {
        let reactionTime = millis() - pingTime;
        socket.emit('reaction', reactionTime);
        reacted = true;
        waiting = false;
        background(0, 255, 0);
        text("¡Reaccionaste!", width / 2, height / 2);
    }
}

function draw() {
    if (!waiting && !reacted) {
        background(200);
        text("Esperando ping...", width / 2, height / 2);
    }
}
```

### `public/page1.html`

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    <script src="/socket.io/socket.io.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/p5.js/1.6.0/p5.js"></script>
    <script src="page1.js"></script>
    <title>Emisor</title>
  </head>
  <body></body>
</html>
```
### `public/page2.html`

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    <script src="/socket.io/socket.io.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/p5.js/1.6.0/p5.js"></script>
    <script src="page2.js"></script>
    <title>Receptor</title>
  </head>
  <body></body>
</html>
```

### `package.json`

```json
{
  "name": "actividad05-ping-reaccion",
  "version": "1.0.0",
  "main": "server.js",
  "dependencies": {
    "express": "^4.18.2",
    "socket.io": "^4.7.2"
  }
}
```

## Reflexión final 

> Lo más fácil fue aprovechar la estructura del caso base. Lo difícil fue lograr una sincronización precisa entre los clientes. Aprendí cómo usar `socket.io` para eventos personalizados entre múltiples usuarios, y cómo hacer apps con lógica compartida en tiempo real. Fue divertido y me ayudó a reforzar conceptos de programación interactiva y comunicación entre clientes.

