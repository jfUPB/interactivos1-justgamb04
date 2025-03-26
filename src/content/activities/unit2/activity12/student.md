### Código en MicroPython (Micro:bit)
```python
from microbit import *
import utime

# Estados de la máquina de estados
STATE_CONFIG = 0
STATE_ARMED = 1
STATE_EXPLODED = 2

# Variables de la bomba
time_left = 20  # Tiempo inicial de 20 segundos
min_time = 10
max_time = 60
current_state = STATE_CONFIG

# Temporizador
start_time = 0
interval = 1000  # 1 segundo en milisegundos

while True:
    if current_state == STATE_CONFIG:
        display.show(str(time_left))  # Mostrar tiempo en la pantalla
        if button_a.was_pressed() and time_left < max_time:
            time_left += 1  # Aumentar tiempo
        if button_b.was_pressed() and time_left > min_time:
            time_left -= 1  # Disminuir tiempo
        if accelerometer.was_gesture("shake"):
            current_state = STATE_ARMED
            start_time = utime.ticks_ms()  # Guardar tiempo de inicio

    elif current_state == STATE_ARMED:
        elapsed_time = utime.ticks_diff(utime.ticks_ms(), start_time)
        if elapsed_time >= interval:
            start_time = utime.ticks_ms()
            time_left -= 1
            display.show(str(time_left))
            if time_left == 0:
                current_state = STATE_EXPLODED  # Cambiar a estado de explosión

    elif current_state == STATE_EXPLODED:
        display.show(Image.SKULL)
        for _ in range(5):
            pin0.write_digital(1)  # Activar buzzer
            sleep(200)
            pin0.write_digital(0)  # Desactivar buzzer
            sleep(200)
        if pin1.read_digital():  # Si se toca el sensor táctil
            current_state = STATE_CONFIG
            time_left = 20
            display.clear()
```

---

#### Código `index.html`
Este archivo crea una simulación visual de la bomba temporizada usando `p5.js`.

```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Bomba Temporizada</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/p5.js/1.4.0/p5.js"></script>
    <script src="sketch.js"></script>
    <style>
        body { text-align: center; font-family: Arial, sans-serif; }
        #controls { margin-top: 20px; }
        button { padding: 10px; margin: 5px; font-size: 16px; }
    </style>
</head>
<body>
    <h1>Bomba Temporizada</h1>
    <canvas id="canvas"></canvas>
    <div id="controls">
        <button onclick="increaseTime()">🔼 Aumentar</button>
        <button onclick="decreaseTime()">🔽 Disminuir</button>
        <button onclick="armBomb()">💣 Armar</button>
        <button onclick="resetBomb()">🔄 Reset</button>
    </div>
</body>
</html>
```

---

#### Código `sketch.js`
Este script simula la cuenta regresiva y la activación de la bomba.

```javascript
let timeLeft = 20;
let minTime = 10;
let maxTime = 60;
let state = "CONFIG";

function setup() {
    createCanvas(400, 300);
    textAlign(CENTER, CENTER);
}

function draw() {
    background(255);
    textSize(32);
    fill(0);

    if (state === "CONFIG") {
        text("Tiempo: " + timeLeft, width / 2, height / 2);
    } else if (state === "ARMED") {
        text("Tiempo: " + timeLeft, width / 2, height / 2);
        if (frameCount % 60 == 0 && timeLeft > 0) {
            timeLeft--;
        }
        if (timeLeft == 0) {
            state = "EXPLODED";
        }
    } else if (state === "EXPLODED") {
        textSize(50);
        fill(255, 0, 0);
        text("💥 BOOM!", width / 2, height / 2);
    }
}

function increaseTime() {
    if (state === "CONFIG" && timeLeft < maxTime) {
        timeLeft++;
    }
}

function decreaseTime() {
    if (state === "CONFIG" && timeLeft > minTime) {
        timeLeft--;
    }
}

function armBomb() {
    if (state === "CONFIG") {
        state = "ARMED";
    }
}

function resetBomb() {
    state = "CONFIG";
    timeLeft = 20;
}
```

---

#### Explicación
- **MicroPython** controla la bomba en el micro:bit con botones y acelerómetro.  
- **HTML y JavaScript (`p5.js`)** crean una interfaz visual interactiva en la web.  
- **Estados manejados:**  
  - `CONFIG`: Ajustar el tiempo con los botones.  
  - `ARMED`: Inicia la cuenta regresiva.  
  - `EXPLODED`: Muestra una explosión en pantalla.  
- **Botones Web**: Simulan los eventos del micro:bit.  
