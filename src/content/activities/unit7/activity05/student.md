## **Propósito**

Modificar una aplicación de partículas para que funcione mediante control remoto: el dispositivo **móvil** enviará la información de interacción (posición, color y estilo), y el **escritorio** la interpretará para generar partículas animadas en tiempo real.

## **Cambios en `mobile/sketch.js`**

### **¿Qué datos envío ahora?**

Además de las coordenadas `x` y `y` del toque, el móvil ahora también envía:

* El color de la partícula (generado aleatoriamente con `r`, `g`, `b`).
* Si la partícula debe tener `stroke` o no (activado con un botón o checkbox).

### **¿Cuándo los envío?**

En cada movimiento del dedo (función `touchMoved()`), se construye un objeto con esta información y se envía por socket.

### **Fragmento clave del código móvil:**

```javascript
let useStroke = false;

function setup() {
  createCanvas(windowWidth, windowHeight);
  background(255);
  let button = createButton('Toggle Stroke');
  button.mousePressed(() => useStroke = !useStroke);
}

function touchMoved() {
  let r = floor(random(255));
  let g = floor(random(255));
  let b = floor(random(255));
  let touchData = {
    type: 'touch',
    x: mouseX,
    y: mouseY,
    color: { r, g, b },
    stroke: useStroke
  };
  socket.emit('message', JSON.stringify(touchData));
  return false;
}
```
## **Cambios en `desktop/sketch.js`**

### **¿Cómo interpreto los datos?**

Al recibir el mensaje del socket, uso `JSON.parse()` para convertirlo en objeto. Si es un mensaje tipo `'touch'`, creo una nueva partícula con los parámetros recibidos.

### **¿Qué modifiqué en `setup()` o `draw()`?**

* Se inicializa el canvas.
* Se añade un array global `particles`.
* En `draw()`, se actualizan y dibujan todas las partículas.
* El color y `stroke` se usan al mostrar cada partícula.

### **Fragmento clave del código de escritorio:**

```javascript
let particles = [];

function setup() {
  createCanvas(windowWidth, windowHeight);
  background(0, 20);
  socket.on('message', (msg) => {
    try {
      const data = JSON.parse(msg);
      if (data.type === 'touch') {
        particles.push(new Particle(data.x, data.y, data.color, data.stroke));
      }
    } catch (e) {
      console.error('Error parsing message:', e);
    }
  });
}

function draw() {
  background(0, 20);
  for (let i = particles.length - 1; i >= 0; i--) {
    particles[i].update();
    particles[i].display();
    if (particles[i].isDead()) particles.splice(i, 1);
  }
}

class Particle {
  constructor(x, y, color, useStroke) {
    this.pos = createVector(x, y);
    this.vel = p5.Vector.random2D();
    this.lifespan = 255;
    this.size = random(5, 20);
    this.noiseOffset = random(1000);
    this.color = color;
    this.useStroke = useStroke;
  }

  update() {
    let angle = noise(this.pos.x * 0.005, this.pos.y * 0.005, this.noiseOffset) * TWO_PI * 2;
    let force = p5.Vector.fromAngle(angle);
    force.mult(0.5);
    this.vel.add(force);
    this.vel.limit(4);
    this.pos.add(this.vel);
    this.lifespan -= 3;
  }

  display() {
    if (this.useStroke) {
      stroke(255);
    } else {
      noStroke();
    }
    fill(this.color.r, this.color.g, this.color.b, this.lifespan);
    ellipse(this.pos.x, this.pos.y, this.size);
  }

  isDead() {
    return this.lifespan < 0;
  }
}
```

## **Cambios en `server.js`**

No fue necesario modificar `server.js`. Su única función es actuar como intermediario entre el móvil y el escritorio, reenviando los mensajes tal como vienen:

```javascript
io.on('connection', (socket) => {
  socket.on('message', (msg) => {
    socket.broadcast.emit('message', msg);
  });
});
```

## **Códigos completos**

### **server.js**

```javascript
const express = require('express');
const app = express();
const server = require('http').Server(app);
const io = require('socket.io')(server);

app.use(express.static('public'));

io.on('connection', (socket) => {
  socket.on('message', (msg) => {
    socket.broadcast.emit('message', msg);
  });
});

server.listen(3000, () => {
  console.log('Servidor corriendo en http://localhost:3000');
});
```

### **mobile/sketch.js**

(incluido arriba)

### **desktop/sketch.js**

(incluido arriba)

## **Enlace al video**

[[**Ver video en YouTube**](https://www.youtube.com/watch?v=EJEMPLO_DEMO_VIDEO)](https://www.youtube.com/watch?v=EJEMPLO_DEM)
