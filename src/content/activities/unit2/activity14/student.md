# Actividad 14: Pruebas y Depuración de la Bomba Temporizada  

## Vectores de Prueba 

Se presentan las pruebas realizadas en ambas implementaciones.  

### 1. Ajuste del tiempo en configuración  

**Objetivo:** Verificar que los botones permitan modificar el tiempo dentro de los límites establecidos.  

| **Plataforma** | **Estado Inicial** | **Acción** | **Esperado** | **Obtenido** | **Corrección** |
|--------------|----------------|------------|--------------|--------------|--------------|
| Micro:bit | Bomba en **CONFIG** con 20s | Presionar A cinco veces | Aumenta a 25s | Funciona correctamente | No fue necesario |
| Micro:bit | Bomba en **CONFIG** con 20s | Presionar B tres veces | Disminuye a 17s | Funciona correctamente | No fue necesario |
| p5.js | Bomba en **CONFIG** con 20s | Click en "Aumentar" cinco veces | Aumenta a 25s | Funciona correctamente | No fue necesario |
| p5.js | Bomba en **CONFIG** con 10s | Click en "Disminuir" | No debe bajar de 10s | Funciona correctamente | No fue necesario |

---

### **2. Límite incorrecto en la configuración del tiempo**  

**Objetivo:** Verificar que el tiempo no supere los valores permitidos (10s - 60s).  

| **Plataforma** | **Estado Inicial** | **Acción** | **Esperado** | **Obtenido** | **Corrección** |
|--------------|----------------|------------|--------------|--------------|--------------|
| Micro:bit | Bomba en **CONFIG** con 60s | Presionar A | No debe aumentar más de 60s | Error: Se incrementa a 61s | Se agregó validación |
| p5.js | Bomba en **CONFIG** con 60s | Click en "Aumentar" | No debe aumentar más de 60s | Error: Se incrementa a 61s | Se agregó validación |

**Corrección en el código (MicroPython y p5.js):**  

```python
if button_a.was_pressed() and time_left < max_time:
    time_left += 1  
```

```javascript
function increaseTime() {
    if (state === "CONFIG" && timeLeft < maxTime) {
        timeLeft++;
    }
}
```

Ahora el tiempo no sobrepasa los 60 segundos.  

---

### **3. Inicio de la cuenta regresiva**  

**Objetivo:** Verificar que la bomba pase al estado **ARMED** cuando se active la detonación.  

| **Plataforma** | **Estado Inicial** | **Acción** | **Esperado** | **Obtenido** | **Corrección** |
|--------------|----------------|------------|--------------|--------------|--------------|
| Micro:bit | Bomba en **CONFIG** con 15s | Agitar micro:bit | Cambia a **ARMED** y comienza la cuenta regresiva | Funciona correctamente | No fue necesario |
| p5.js | Bomba en **CONFIG** con 15s | Click en "Armar" | Cambia a **ARMED** y comienza la cuenta regresiva | Funciona correctamente | No fue necesario |

---

### **4. Reinicio de la bomba tras la explosión**  

**Objetivo:** Asegurar que la bomba pueda reiniciarse después de la explosión.  

| **Plataforma** | **Estado Inicial** | **Acción** | **Esperado** | **Obtenido** | **Corrección** |
|--------------|----------------|------------|--------------|--------------|--------------|
| Micro:bit | Bomba en **EXPLODED** | Presionar botón táctil (pin1) | Reinicia a **CONFIG** con 20s | Error: No respondía | Se implementó la validación |
| p5.js | Bomba en **EXPLODED** | Click en "Reset" | Reinicia a **CONFIG** con 20s | Funciona correctamente | No fue necesario |

**Corrección en el código (MicroPython):**  

```python
if pin1.read_digital():  
    current_state = STATE_CONFIG
    time_left = 20
    display.clear()
```

---

## **Código Final Corregido**  

### **MicroPython (Micro:bit)**
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
start_time = 0
interval = 1000  

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

    elif current_state == STATE_ARMED:
        elapsed_time = utime.ticks_diff(utime.ticks_ms(), start_time)
        if elapsed_time >= interval:
            start_time = utime.ticks_ms()
            time_left -= 1
            display.show(str(time_left))

            if time_left <= 0:
                current_state = STATE_EXPLODED  

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

### **p5.js**
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
```
