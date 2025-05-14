### **Código: `sketch.js` (p5.js)**
```javascript
let bombaActiva = false; // Estado de la bomba
let tiempo = 10; // Tiempo inicial
let serial; // Objeto para comunicación serial
let receivedData = ""; // Variable para datos recibidos

function setup() {
  createCanvas(400, 400);
  serial = new p5.SerialPort(); // Crear puerto serial
  serial.list(); // Listar puertos disponibles
  serial.open("/dev/ttyUSB0"); // Ajusta el puerto según tu sistema
  serial.on("data", serialEvent); // Escuchar datos del micro:bit
}

function draw() {
  background(220);

  // Dibujar bomba
  fill(50);
  ellipse(200, 200, 100, 100);
  
  // Mecha
  stroke(0);
  strokeWeight(3);
  line(200, 150, 200, 120);
  fill(255, 100, 0);
  ellipse(200, 115, 10, 10);

  // Mostrar estado de la bomba
  noStroke();
  fill(0);
  textAlign(CENTER, CENTER);
  textSize(20);
  text(bombaActiva ? "¡ACTIVADA!" : "INACTIVA", 200, 250);

  if (bombaActiva) {
    textSize(16);
    text("Tiempo: " + tiempo, 200, 280);
  }
}

// Función para recibir datos del micro:bit
function serialEvent() {
  receivedData = serial.readLine().trim();
  if (receivedData === "A") { // Botón A activa la bomba
    bombaActiva = true;
  } else if (receivedData === "B") { // Botón B desactiva la bomba
    bombaActiva = false;
  } else if (receivedData === "S" && bombaActiva) { // Shake disminuye el tiempo
    tiempo--;
    if (tiempo <= 0) {
      bombaActiva = false;
      tiempo = 10; // Reiniciar
    }
  }
}
```
### Código: micro:bit
```python
from microbit import *
import utime

while True:
    if button_a.is_pressed():
        uart.write("A\n")  # Enviar evento de activación
        utime.sleep(0.5)

    if button_b.is_pressed():
        uart.write("B\n")  # Enviar evento de desactivación
        utime.sleep(0.5)

    if accelerometer.was_gesture("shake"):
        uart.write("S\n")  # Enviar evento de agitar
        utime.sleep(0.5)
```

### **Explicación del código:**
1. **p5.js**
   - Se establece **comunicación serial** con el micro:bit.
   - Se **dibuja la bomba** y su estado.
   - Se **escuchan eventos** del micro:bit:
     - `"A"` → **Activa la bomba**.
     - `"B"` → **Desactiva la bomba**.
     - `"S"` → **Reduce el tiempo hasta explotar**.

2. **MicroPython**
   - Se usan **botón A, botón B y shake** para enviar eventos al **puerto serial**.
