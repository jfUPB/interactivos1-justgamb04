### **`sketch.js`**  
```javascript
let serial;
let portName = "/dev/ttyUSB0"; // Ajusta esto según tu sistema
let timeLeft = 20;
let minTime = 10;
let maxTime = 60;
let state = "CONFIG";

function setup() {
    createCanvas(400, 300);
    textAlign(CENTER, CENTER);

    // Configurar comunicación serial
    serial = new p5.SerialPort();
    serial.list();
    serial.open(portName);
}

function draw() {
    background(255);
    textSize(32);
    fill(0);

    if (state === "CONFIG") {
        text("Tiempo: " + timeLeft, width / 2, height / 2);
    } else if (state === "ARMED") {
        text("Tiempo: " + timeLeft, width / 2, height / 2);
        if (millis() % 1000 < 50 && timeLeft > 0) {
            timeLeft--;
        }
        if (timeLeft <= 0) {
            state = "EXPLODED";
        }
    } else if (state === "EXPLODED") {
        textSize(50);
        fill(255, 0, 0);
        text("BOOM", width / 2, height / 2);
    }
}

function sendEvent(event) {
    serial.write(event + "\n"); // Enviar evento al Micro:bit
}
```

### **`index.html` (Botones para enviar comandos)**  
```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Control de Bomba</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/p5.js/1.4.0/p5.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/p5.js/1.4.0/addons/p5.serialport.js"></script>
    <script src="sketch.js"></script>
</head>
<body>
    <h1>Control de la Bomba</h1>
    <button onclick="sendEvent('A')">Botón A</button>
    <button onclick="sendEvent('B')">Botón B</button>
    <button onclick="sendEvent('S')">Shake</button>
    <button onclick="sendEvent('T')">Botón Touch</button>
    <button onclick="sendEvent('RESET')">Reiniciar</button>
</body>
</html>
```
### **Explicación de las mejoras**  
1. **Comunicación Serial**  
   - `serial = new p5.SerialPort();` → Crea un objeto serial.  
   - `serial.list();` → Lista los puertos disponibles en la consola.  
   - `serial.open(portName);` → Abre el puerto **(ajusta el nombre según tu sistema)**.  
   - `serial.write(event + "\n");` → Envía eventos como texto al **Micro:bit**.  

2. **Botones en `index.html`**  
   - Cada botón ahora llama a `sendEvent('X')`, que envía la letra correspondiente por serial.  

### **MicroPython (Código para recibir comandos en Micro:bit)**  
Este código debe correr en el **Micro:bit** para recibir y procesar los mensajes desde **p5.js**.  
```python
from microbit import *
import utime

STATE_CONFIG = 0
STATE_ARMED = 1
STATE_EXPLODED = 2

time_left = 20  
min_time = 10
max_time = 60
current_state = STATE_CONFIG

def handle_serial():
    global current_state, time_left
    if uart.any():
        event = uart.read().strip().decode("utf-8")
        if current_state == STATE_CONFIG:
            if event == "A" and time_left < max_time:
                time_left += 1
            elif event == "B" and time_left > min_time:
                time_left -= 1
            elif event == "S":
                current_state = STATE_ARMED
        elif current_state == STATE_ARMED:
            if event == "T":
                current_state = STATE_CONFIG
                time_left = 20
        elif current_state == STATE_EXPLODED:
            if event == "RESET":
                current_state = STATE_CONFIG
                time_left = 20

# Configurar Serial
uart.init(115200)

while True:
    handle_serial()
    if current_state == STATE_CONFIG:
        display.show(str(time_left))
    elif current_state == STATE_ARMED:
        display.show(str(time_left))
        utime.sleep(1)
        time_left -= 1
        if time_left <= 0:
            current_state = STATE_EXPLODED
    elif current_state == STATE_EXPLODED:
        display.show(Image.SKULL)
```

### **Resumen**  
Ahora la **bomba se controla desde p5.js** y envía eventos por **serial** al Micro:bit.  
Se usa `p5.serialport.js` para manejar la conexión.  
**MicroPython** procesa los eventos recibidos por **UART**.  
