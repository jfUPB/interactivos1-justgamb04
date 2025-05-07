## **Explicación de la implementación**  

En esta versión, la bomba ahora puede ser controlada desde dos fuentes diferentes:  

1. **Desde el Micro:bit**, usando los botones y sensores.  
2. **Desde el puerto serial**, recibiendo comandos de un programa externo.  

Para lograr esto, se hizo una refactorización del código usando el concepto de **eventos**. Ahora, en vez de que la máquina de estados lea directamente los botones o sensores, **espera recibir eventos** que pueden venir de cualquier fuente.  

### **¿Cómo funciona el código?**  

- Se creó una **variable `evento_recibido`** que indica si ocurrió un evento.  
- Otra variable, **`evento_actual`**, almacena qué evento ocurrió (por ejemplo, `"A"` si se presionó el botón A o si se recibió el carácter `"A"` por el puerto serial).  
- Hay dos funciones principales:  

  1. `tareaEventos()`:  
     - Se encarga de detectar los eventos tanto del Micro:bit como del puerto serial.  
     - Si detecta que se presionó un botón o se recibió un comando por el serial, actualiza `evento_actual` y pone `evento_recibido = True`.  

  2. `tareaBomba()`:  
     - Lee el evento registrado y lo procesa según el estado actual de la bomba.  
     - Luego, **consume el evento**, es decir, pone `evento_recibido = False` para esperar un nuevo evento.  

## **Código en Micro:bit**  

```python
from microbit import *
import utime

# Estados de la bomba
STATE_CONFIG = 0
STATE_ARMED = 1
STATE_EXPLODED = 2

# Variables de la bomba
time_left = 20  
min_time = 10
max_time = 60
current_state = STATE_CONFIG
start_time = 0
interval = 1000  

# Variables de eventos
evento_recibido = False
evento_actual = ""

# Secuencia para desactivar la bomba
disarm_sequence = ["A", "B", "A", "S"]
user_input = []

# Función que maneja los eventos
def tareaEventos():
    global evento_recibido, evento_actual
    
    # Verificar botones del Micro:bit
    if button_a.was_pressed():
        evento_actual = "A"
        evento_recibido = True
    elif button_b.was_pressed():
        evento_actual = "B"
        evento_recibido = True
    elif accelerometer.was_gesture("shake"):
        evento_actual = "S"
        evento_recibido = True
    elif pin_logo.is_touched():
        evento_actual = "T"
        evento_recibido = True

    # Leer eventos desde el puerto serial
    if uart.any():
        recibido = uart.read().decode().strip()
        if recibido in ["A", "B", "S", "T"]:
            evento_actual = recibido
            evento_recibido = True

# Función que maneja la lógica de la bomba
def tareaBomba():
    global time_left, current_state, start_time, evento_recibido, evento_actual, user_input

    if current_state == STATE_CONFIG:
        display.show(str(time_left))  
        
        if evento_recibido:
            if evento_actual == "A" and time_left < max_time:
                time_left += 1
            elif evento_actual == "B" and time_left > min_time:
                time_left -= 1
            elif evento_actual == "S":  
                current_state = STATE_ARMED
                start_time = utime.ticks_ms()
                user_input = []
        
        evento_recibido = False  

    elif current_state == STATE_ARMED:
        elapsed_time = utime.ticks_diff(utime.ticks_ms(), start_time)
        if elapsed_time >= interval:
            start_time = utime.ticks_ms()
            time_left -= 1
            display.show(str(time_left))

            if time_left <= 0:
                current_state = STATE_EXPLODED  

        if evento_recibido:
            user_input.append(evento_actual)

            # Verificar si la secuencia de desactivación es correcta
            if user_input == disarm_sequence:
                current_state = STATE_CONFIG
                time_left = 20
                display.clear()
                user_input = []
            elif len(user_input) > len(disarm_sequence):
                user_input = []  

        evento_recibido = False  

    elif current_state == STATE_EXPLODED:
        display.show(Image.SKULL)
        for _ in range(5):
            pin0.write_digital(1)  
            sleep(200)
            pin0.write_digital(0)  
            sleep(200)
        
        if evento_recibido and evento_actual == "T":  
            current_state = STATE_CONFIG
            time_left = 20
            display.clear()

        evento_recibido = False  

# Inicializar comunicación serial
uart.init(baudrate=115200)

# Bucle principal
while True:
    tareaBomba()
    tareaEventos()
```

### **`index.html`**
```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Simulación de Bomba</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/p5.js/1.4.0/p5.js"></script>
    <script src="sketch.js"></script>
</head>
<body>
    <h1>Simulación de Bomba</h1>
    <button onclick="sendEvent('A')">Botón A</button>
    <button onclick="sendEvent('B')">Botón B</button>
    <button onclick="sendEvent('S')">Shake</button>
    <button onclick="sendEvent('T')">Botón Touch</button>
    <button onclick="sendEvent('RESET')">Reiniciar</button>
</body>
</html>
```

### **`sketch.js`**
```javascript
let timeLeft = 20;
let minTime = 10;
let maxTime = 60;
let state = "CONFIG";
let eventTriggered = false;
let lastEvent = "";

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

    processEvent();
}

function sendEvent(event) {
    eventTriggered = true;
    lastEvent = event;
}

function processEvent() {
    if (eventTriggered) {
        if (state === "CONFIG") {
            if (lastEvent === "A" && timeLeft < maxTime) timeLeft++;
            if (lastEvent === "B" && timeLeft > minTime) timeLeft--;
            if (lastEvent === "S") state = "ARMED";
        } else if (state === "ARMED") {
            if (lastEvent === "T") {
                state = "CONFIG";
                timeLeft = 20;
            }
        } else if (state === "EXPLODED") {
            if (lastEvent === "RESET") {
                state = "CONFIG";
                timeLeft = 20;
            }
        }
        eventTriggered = false;
        lastEvent = "";
    }
}
```

## **Conclusión**  

En esta versión, la bomba ahora puede recibir comandos tanto del Micro:bit como del puerto serial sin que la máquina de estados sepa de dónde vienen los eventos.  

Esto lo logramos separando la lógica de detección de eventos en `tareaEventos()` y la lógica de la bomba en `tareaBomba()`.  

De esta forma, la bomba puede ser armada, configurada o desactivada tanto de forma física (Micro:bit) como digital (puerto serial), lo que la hace mucho más flexible y funcional.
