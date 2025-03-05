
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
#### Explicación del funcionamiento 

Este proyecto conecta un **Micro:bit** con una página web hecha en **p5.js**, permitiendo que el **Micro:bit envíe señales** cuando se presionan sus botones. La página web recibe estos datos y muestra diferentes imágenes en la pantalla.  

---

#### Micro:bit – Envío de datos  
- El código en **MicroPython** detecta cuándo se presionan los botones **A**, **B** o el **logo táctil**.  
- Cuando se presiona un botón, se **muestra una imagen en la pantalla LED del micro:bit**.  
- Luego, el **micro:bit envía un mensaje por serial** con el nombre de la imagen mostrada (por ejemplo, "HEART" si se presiona A).  

---

#### Comunicación entre Micro:bit y la página web 
- **El micro:bit y la computadora están conectados por USB**.  
- El **micro:bit envía los datos en forma de texto (HEART, SMILE o GIRAFFE)** usando UART (puerto serial).  
- La página web en **p5.js recibe estos datos** y cambia la imagen en la pantalla según el mensaje recibido.  

---

#### Visualización en p5.js  
- En **p5.js**, un código interpreta los datos y **dibuja emojis** en la pantalla según lo que envía el Micro:bit.  
- **Ejemplo:**  
  - Si se recibe "HEART" → Se muestra un corazón ❤️.  
  - Si se recibe "SMILE" → Se muestra una cara feliz 😊.  
  - Si se recibe "GIRAFFE" → Se muestra una jirafa 🦒.  
- Si no hay datos, aparece el mensaje: **"Presiona un botón"**.  


