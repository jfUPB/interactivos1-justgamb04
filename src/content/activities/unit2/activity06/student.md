
---

#### Código en MicroPython (micro:bit)


```python
from microbit import *
import utime

while True:
    if button_a.is_pressed():
        display.show(Image.HEART)
        uart.write("HEART\n")  # Enviar comando por serial
        utime.sleep(1)
        
    elif button_b.is_pressed():
        display.show(Image.SMILE)
        uart.write("SMILE\n")  # Enviar comando por serial
        utime.sleep(1)

    elif pin_logo.is_touched():
        display.show(Image.GIRAFFE)
        uart.write("GIRAFFE\n")  # Enviar comando por serial
        utime.sleep(1)
```

---

## Código en `index.html`

```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Micro:bit y p5.js</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/p5.js/1.4.0/p5.min.js"></script>
</head>
<body>
    <h1>Visualización de Micro:bit</h1>
    <script src="sketch.js"></script>
</body>
</html>
```

---

#### Código en `sketch.js`

```javascript
let receivedData = "";  // Variable para recibir los datos

function setup() {
    createCanvas(400, 400);
    background(220);
}

function draw() {
    background(220);
    textSize(32);
    textAlign(CENTER, CENTER);
    
    if (receivedData === "HEART") {
        text("❤️", width / 2, height / 2);
    } else if (receivedData === "SMILE") {
        text("😊", width / 2, height / 2);
    } else if (receivedData === "GIRAFFE") {
        text("🦒", width / 2, height / 2);
    } else {
        text("Presiona un botón", width / 2, height / 2);
    }
}

// Simulación de recepción de datos (Para pruebas sin conexión serial)
function keyPressed() {
    if (key === 'A') {
        receivedData = "HEART";
    } else if (key === 'B') {
        receivedData = "SMILE";
    } else if (key === 'L') {
        receivedData = "GIRAFFE";
    }
}
```
## **Explicación del funcionamiento**  

Este proyecto conecta el **Micro:bit** con una página web usando **p5.js**. La idea es que cuando se presionen los botones del micro:bit, aparezcan diferentes imágenes en la pantalla LED y también se envíe un mensaje a la página web para que muestre algo relacionado.  

---

### **1. Código en el Micro:bit**  
En el micro:bit, se programan los botones **A**, **B** y el **logo táctil** para que muestren diferentes imágenes en la pantalla LED. Además, cuando se presiona uno de estos botones, el micro:bit **envía un mensaje por el puerto serial** con el nombre de la imagen que mostró, por ejemplo, "HEART" si se presiona A.  

---

### **2. Comunicación entre Micro:bit y la página web**  
El micro:bit está **conectado por USB** a la computadora. Cuando se presiona un botón, se envía un mensaje por el puerto serial, que la página web en **p5.js recibe** y usa para cambiar lo que muestra en la pantalla.  

---

### **3. Lo que pasa en la página web (p5.js)**  
El código en p5.js escucha los mensajes que manda el micro:bit y, según el mensaje recibido, **dibuja una imagen en la pantalla**.  

Por ejemplo:  
- Si llega el mensaje **"HEART"**, se muestra un corazón.  
- Si llega **"SMILE"**, se dibuja una cara feliz.  
- Si llega **"GIRAFFE"**, se muestra una jirafa.  
- Si no hay datos, aparece un mensaje diciendo **"Presiona un botón"**.  

---
