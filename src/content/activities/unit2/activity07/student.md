### **Código para el Micro:bit (Python)**

```python
from microbit import *
import music

while True:
    if button_a.is_pressed():
        uart.write("A\n")  # Envía la letra "A" por el puerto serial
        music.play(music.POWER_UP)  # Suena un tono al presionar A
        sleep(300)

    if button_b.is_pressed():
        uart.write("B\n")  # Envía la letra "B" por el puerto serial
        music.play(music.POWER_DOWN)  # Suena un tono al presionar B
        sleep(300)
```

---

### **Código para `index.html`**

```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Micro:bit y Sonidos</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/p5.js/1.4.0/p5.js"></script>
    <script src="sketch.js"></script>
</head>
<body>
    <h1>Control de Sonidos con Micro:bit</h1>
    <button id="connect">Conectar Micro:bit</button>
</body>
</html>
```

---

### **Código para `sketch.js`**

```javascript
let port;
let reader;

function setup() {
    noCanvas();
    let button = select("#connect");
    button.mousePressed(connectMicrobit);
}

async function connectMicrobit() {
    try {
        port = await navigator.serial.requestPort();
        await port.open({ baudRate: 115200 });
        reader = port.readable.getReader();
        readSerialData();
    } catch (err) {
        console.error("Error al conectar con el Micro:bit:", err);
    }
}

async function readSerialData() {
    while (true) {
        const { value, done } = await reader.read();
        if (done) break;
        
        let receivedData = new TextDecoder().decode(value).trim();
        console.log("Dato recibido:", receivedData);

        if (receivedData === "A") {
            playSound(440); // Sonido para botón A
        } else if (receivedData === "B") {
            playSound(220); // Sonido para botón B
        }
    }
}

function playSound(frequency) {
    let osc = new (window.AudioContext || window.webkitAudioContext)();
    let oscillator = osc.createOscillator();
    oscillator.type = "sine";
    oscillator.frequency.setValueAtTime(frequency, osc.currentTime);
    oscillator.connect(osc.destination);
    oscillator.start();
    setTimeout(() => oscillator.stop(), 500);
}
```

---

### **Explicación del Funcionamiento**
#### 1. Micro:bit (Python)
   -Detecta si se presiona el botón **A** o **B**.
   -Reproduce un sonido con `music.play()`.
   -Envía el dato `"A"` o `"B"` por **puerto serial** con `uart.write()`.

#### 2. p5.js (sketch.js)
   -Conecta el micro:bit a través del puerto serial cuando el usuario presiona el botón en la web.
   -Lee los datos seriales y detecta si recibe `"A"` o `"B"`.
   -Genera sonidos- en la web con `playSound(frequency)`, donde cambia la frecuencia según el botón presionado.

#### 3. index.html
-Contiene un botón para conectar el micro:bit y visualizarlo.

---

