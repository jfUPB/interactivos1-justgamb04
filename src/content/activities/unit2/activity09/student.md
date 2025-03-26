#### Código para Micro:bit (MicroPython)
```python
from microbit import *
import utime

class Semaforo:
    def __init__(self):
        self.estado = "Rojo"
        self.tiempo_inicio = utime.ticks_ms()

    def actualizar(self):
        tiempo_actual = utime.ticks_ms()
        
        if self.estado == "Rojo":
            display.show(Image.HEART)  # Simula la luz roja
            if utime.ticks_diff(tiempo_actual, self.tiempo_inicio) > 3000:  # 3 segundos
                self.estado = "Verde"
                self.tiempo_inicio = utime.ticks_ms()

        elif self.estado == "Verde":
            display.show(Image.YES)  # Simula la luz verde
            if utime.ticks_diff(tiempo_actual, self.tiempo_inicio) > 3000:  # 3 segundos
                self.estado = "Amarillo"
                self.tiempo_inicio = utime.ticks_ms()

        elif self.estado == "Amarillo":
            display.show(Image.ARROW_N)  # Simula la luz amarilla
            if utime.ticks_diff(tiempo_actual, self.tiempo_inicio) > 1500:  # 1.5 segundos
                self.estado = "Rojo"
                self.tiempo_inicio = utime.ticks_ms()

semaforo = Semaforo()

while True:
    semaforo.actualizar()
    sleep(100)
```

---

#### Código para `index.html`
```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Simulación de Semáforo</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/p5.js/1.5.0/p5.js"></script>
    <script src="sketch.js"></script>
</head>
<body>
    <h1>Semáforo en p5.js</h1>
    <p>Simulación de un semáforo con máquina de estados.</p>
</body>
</html>
```

---

#### Código para `sketch.js`
```javascript
let estado = "Rojo";
let tiempoInicio;

function setup() {
    createCanvas(200, 400);
    tiempoInicio = millis();
}

function draw() {
    background(220);
    fill(50);
    rect(75, 50, 50, 300); // Poste del semáforo

    let tiempoActual = millis();
    
    if (estado === "Rojo") {
        fill(255, 0, 0);
        ellipse(100, 100, 50, 50);
        if (tiempoActual - tiempoInicio > 3000) {
            estado = "Verde";
            tiempoInicio = millis();
        }
    } else if (estado === "Verde") {
        fill(0, 255, 0);
        ellipse(100, 200, 50, 50);
        if (tiempoActual - tiempoInicio > 3000) {
            estado = "Amarillo";
            tiempoInicio = millis();
        }
    } else if (estado === "Amarillo") {
        fill(255, 255, 0);
        ellipse(100, 300, 50, 50);
        if (tiempoActual - tiempoInicio > 1500) {
            estado = "Rojo";
            tiempoInicio = millis();
        }
    }
}
```

---

#### Explicación del Código
Para esta actividad, lo que hice fue implementar un semáforo simple que cambia de color con el tiempo usando una **máquina de estados** tanto en el micro:bit como en p5.js.

1. **Código en MicroPython (para el micro:bit)**  
   - Creé una clase `Semaforo` que maneja los tres estados: **Rojo**, **Verde** y **Amarillo**.
   - Usé `utime.ticks_ms()` para contar el tiempo y cambiar de estado después de unos segundos.
   - Utilicé imágenes prediseñadas en la pantalla LED para representar cada luz del semáforo.

2. **Código en p5.js (para la simulación en la web)**  
   - Creé una estructura simple en `index.html` para visualizar la simulación.
   - En `sketch.js`, usé `ellipse()` para dibujar los colores del semáforo y `millis()` para controlar el tiempo de cada estado.
   - Implementé la lógica de la máquina de estados para que el color cambie de manera automática.
