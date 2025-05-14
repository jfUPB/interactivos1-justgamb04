
### **Código: `sketch.js`**
```javascript
let bombaActiva = false; // Estado de la bomba
let tiempo = 10; // Tiempo inicial de la bomba

function setup() {
  createCanvas(400, 400);
}

function draw() {
  background(220);
  
  // Dibujar la bomba
  fill(50);
  ellipse(200, 200, 100, 100); // Cuerpo de la bomba
  
  // Mecha de la bomba
  stroke(0);
  strokeWeight(3);
  line(200, 150, 200, 120); // Línea de la mecha
  fill(255, 100, 0);
  ellipse(200, 115, 10, 10); // Fuego en la mecha
  
  // Texto para el estado de la bomba
  noStroke();
  fill(0);
  textAlign(CENTER, CENTER);
  textSize(20);
  text(bombaActiva ? "¡ACTIVADA!" : "INACTIVA", 200, 250);
  
  // Mostrar tiempo si la bomba está activa
  if (bombaActiva) {
    textSize(16);
    text("Tiempo: " + tiempo, 200, 280);
  }
}
```
