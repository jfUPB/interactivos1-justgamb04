![image](https://github.com/user-attachments/assets/8f3d7508-2d0c-4a10-a6d1-965e7cd41b01)
![image](https://github.com/user-attachments/assets/e495254d-055b-47f5-b9c3-89e141223906)
![image](https://github.com/user-attachments/assets/ebb447af-5441-41f1-9816-0f2c9747c4c1)
![image](https://github.com/user-attachments/assets/ed8acaef-6660-4606-8d0f-a92f46bdac99)



#### Pasos del Desarrollo 

##### Configuración del Micro:bit (MicroPython)
Para que el Micro:bit pueda recibir datos desde la computadora, activamos la comunicación serial (`uart`). Luego, el código escucha los datos entrantes y los muestra en la pantalla LED.  

##### Código usado en microbit: 
```python
from microbit import *

uart.init(baudrate=115200)  # Inicializa la comunicación serial

while True:
    if uart.any():  # Verifica si hay datos en el puerto serial
        dato = uart.read(1)  # Lee un solo byte
        if dato:  # Verifica que no sea None
            display.show(dato.decode('utf-8'))  # Muestra el carácter en la pantalla LED
```
#### Explicación:
- `uart.init(baudrate=115200)`: Inicia la comunicación serial a 115200 baudios.  
- `uart.any()`: Verifica si hay datos entrantes.  
- `uart.read(1)`: Lee un solo carácter.  
- `dato.decode('utf-8')`: Convierte el byte recibido en un texto legible.  

---

#### Creación del Código en p5.js
El código en p5.js crea una interfaz donde el usuario puede conectar el Micro:bit y enviar teclas presionadas a través de **Web Serial API**.

##### Código para `sketch.js`: 
```javascript
let port;
let writer;

function setup() {
  createCanvas(400, 200);
  background(220);

  let connectButton = createButton("Conectar Micro:bit");
  connectButton.position(10, 10);
  connectButton.mousePressed(connectMicrobit);
}

function draw() {
  textSize(20);
  textAlign(CENTER, CENTER);
  text("Presiona una tecla para enviarla al Micro:bit", width / 2, height / 2);
}

async function connectMicrobit() {
  try {
    port = await navigator.serial.requestPort();  // Solicita permiso al usuario
    await port.open({ baudRate: 115200 });  // Abre el puerto con la velocidad adecuada

    writer = port.writable.getWriter();
    console.log("Micro:bit conectado!");
  } catch (error) {
    console.error("Error al conectar:", error);
  }
}

function keyPressed() {
  if (writer) {
    let keyToSend = key;  // Toma la tecla presionada
    writer.write(new TextEncoder().encode(keyToSend));  // La envía al Micro:bit
    console.log("Enviando:", keyToSend);
  } else {
    console.log("Micro:bit no conectado.");
  }
}
```

#### Explicación:
- `navigator.serial.requestPort()`: Abre la ventana de selección del puerto serie.  
- `port.open({ baudRate: 115200 })`: Establece la conexión con la velocidad adecuada.  
- `writer.write(new TextEncoder().encode(key))`: Envía la tecla presionada.  
- `console.log("Enviando:", key)`: Muestra en la consola qué tecla se envió.  

---

#### Creación del Archivo `index.html`  
El archivo HTML carga el código de p5.js y permite la ejecución en un navegador.

#### Código para `index.html`:  
```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Comunicación p5.js - Micro:bit</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/p5.js/1.4.0/p5.js"></script>
    <script src="sketch.js"></script>
</head>
<body>
</body>
</html>
```



#### Resultados Obtenidos  
Después de implementar los códigos, realizamos pruebas para confirmar que el sistema funciona correctamente:

##### - Cuando presionamos una tecla en p5.js, se muestra en el Micro:bit.  
##### - El botón "Conectar Micro:bit" permite enlazar el dispositivo sin errores.  
##### - Ya no aparece el error `p5.SerialPort is not a constructor`, ya que usamos Web Serial API.  



