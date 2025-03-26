## **Explicación de la implementación**  

Bueno, en esta versión de la bomba lo que hicimos fue agregar una funcionalidad nueva, ahora se puede desactivar si se ingresa la secuencia correcta. La secuencia que hay que seguir es:  

**Botón A → Botón B → Botón A → Shake**  

Si se hace bien, la bomba se desactiva y vuelve a modo configuración, pero si se hace mal o no se alcanza a hacer a tiempo, la bomba sigue contando hasta que explota.  

El código se hizo en dos versiones:  

1. **MicroPython (Micro:bit)** → Esta es la versión que usa los botones y sensores del Micro:bit.  
2. **p5.js (HTML + JavaScript)** → Esta es una simulación en una página web que muestra la bomba funcionando.  

## **Código en (Micro:bit)**  

```python
from microbit import *
import utime

# Estados de la bomba
STATE_CONFIG = 0
STATE_ARMED = 1
STATE_EXPLODED = 2

time_left = 20  
min_time = 10
max_time = 60
current_state = STATE_CONFIG
start_time = 0
interval = 1000  

# Secuencia para desactivar la bomba
disarm_sequence = ["A", "B", "A", "SHAKE"]
user_input = []

while True:
    if current_state == STATE_CONFIG:
        display.show(str(time_left))  
        
        if button_a.was_pressed() and time_left < max_time:
            time_left += 1  
        
        if button_b.was_pressed() and time_left > min_time:
            time_left -= 1  

        if accelerometer.was_gesture("shake"):
            current_state = STATE_ARMED
            start_time = utime.ticks_ms()
            user_input = []  

    elif current_state == STATE_ARMED:
        elapsed_time = utime.ticks_diff(utime.ticks_ms(), start_time)
        if elapsed_time >= interval:
            start_time = utime.ticks_ms()
            time_left -= 1
            display.show(str(time_left))

            if time_left <= 0:
                current_state = STATE_EXPLODED  

        # Capturar la secuencia para desactivar
        if button_a.was_pressed():
            user_input.append("A")
        elif button_b.was_pressed():
            user_input.append("B")
        elif accelerometer.was_gesture("shake"):
            user_input.append("SHAKE")

        # Revisar si la secuencia es correcta
        if user_input == disarm_sequence:
            current_state = STATE_CONFIG
            time_left = 20
            display.clear()
            user_input = []
        elif len(user_input) > len(disarm_sequence):
            user_input = []  

        # Reset después de que explote
        if pin1.read_digital():  
            current_state = STATE_CONFIG
            time_left = 20
            display.clear()

    elif current_state == STATE_EXPLODED:
        display.show(Image.SKULL)
        for _ in range(5):
            pin0.write_digital(1)  
            sleep(200)
            pin0.write_digital(0)  
            sleep(200)
        
        if pin1.read_digital():  
            current_state = STATE_CONFIG
            time_left = 20
            display.clear()
```

## **Código en p5.js**  

```javascript
let timeLeft = 20;
let minTime = 10;
let maxTime = 60;
let state = "CONFIG"; // Estados: CONFIG, ARMED, EXPLODED
let disarmSequence = ["A", "B", "A", "SHAKE"];
let userInput = [];
let lastShakeTime = 0;

function setup() {
    createCanvas(400, 300);
    textAlign(CENTER, CENTER);
    let armButton = createButton("Armar");
    armButton.mousePressed(armBomb);
    let resetButton = createButton("Reset");
    resetButton.mousePressed(resetBomb);
    let buttonA = createButton("A");
    buttonA.mousePressed(() => handleInput("A"));
    let buttonB = createButton("B");
    buttonB.mousePressed(() => handleInput("B"));
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
        if (timeLeft <= 0) {
            state = "EXPLODED";
        }
    } else if (state === "EXPLODED") {
        textSize(50);
        fill(255, 0, 0);
        text("BOOM", width / 2, height / 2);
    }
}

// Función para activar la bomba
function armBomb() {
    if (state === "CONFIG") {
        state = "ARMED";
        userInput = [];
    }
}

// Función para resetear la bomba después de explotar
function resetBomb() {
    if (state === "EXPLODED") {
        state = "CONFIG";
        timeLeft = 20;
    }
}

// Función para manejar la entrada de los botones
function handleInput(input) {
    if (state === "ARMED") {
        userInput.push(input);
        checkDisarmSequence();
    }
}

// Simular el "shake" con la tecla "S"
function keyPressed() {
    if (key === "S" || key === "s") {
        let currentTime = millis();
        if (currentTime - lastShakeTime > 500) {
            userInput.push("SHAKE");
            lastShakeTime = currentTime;
            checkDisarmSequence();
        }
    }
}

// Verificar si la secuencia de desactivación es correcta
function checkDisarmSequence() {
    if (JSON.stringify(userInput) === JSON.stringify(disarmSequence)) {
        state = "CONFIG";
        timeLeft = 20;
        userInput = [];
    } else if (userInput.length > disarmSequence.length) {
        userInput = []; // Reiniciar si la secuencia es incorrecta
    }
}
```

## **Conclusión**  

En esta versión mejoramos la bomba agregando la función de **desactivación**, para que no solo explote, sino que el usuario tenga la oportunidad de desarmarla con una secuencia específica.  

Para hacerlo, en el código de **MicroPython**, usamos botones y el acelerómetro del Micro:bit para detectar la secuencia correcta. Mientras que en **p5.js**, usamos botones HTML y la tecla `"S"` para simular el shake.  

Si la secuencia se ingresa bien, la bomba vuelve a modo configuración. Si no, sigue contando hasta explotar. Esto hace que el código sea más interactivo y permite entender mejor cómo manejar estados y entradas del usuario en diferentes plataformas.
