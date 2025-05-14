### Código del micro:bit

```python
from microbit import *
import struct

uart.init(115200)
display.set_pixel(0, 0, 9)

while True:
    xValue = accelerometer.get_x()
    yValue = accelerometer.get_y()
    aState = button_a.is_pressed()
    bState = button_b.is_pressed()
    # Empaqueta los datos: 2 enteros (16 bits) y 2 bytes para estados
    data = struct.pack('>2h2B', xValue, yValue, int(aState), int(bState))
    # Calcula un checksum simple: suma de los bytes de data módulo 256
    checksum = sum(data) % 256
    # Crea el paquete con header, datos y checksum
    packet = b'\xAA' + data + bytes([checksum])
    uart.write(packet)
    sleep(100)  # Envía datos a 10 Hz
```

Este código ya empaqueta los datos correctamente y los envía a través de UART a la aplicación p5.js.

### Código de p5.js para recibir y procesar los datos binarios:

https://editor.p5js.org/just_gamb04/sketches/0Ff7eqh0i

```javascript
let serial; // objeto de conexión serial
let portName = '/dev/ttyUSB0'; // Cambiar por el puerto adecuado para tu sistema

// Variables para almacenar los datos
let xValue, yValue, aState, bState;

function setup() {
  createCanvas(400, 400);
  serial = new p5.SerialPort();
  serial.open(portName);
  serial.on('data', gotData);
}

function gotData() {
  let data = serial.read(); // Lee un byte

  // Aquí procesamos el paquete binario
  if (data === 0xAA) { // Header
    let packet = serial.read(6); // Lee los 6 bytes siguientes

    // Extraemos los datos de los 6 bytes
    xValue = packet[0] << 8 | packet[1];  // 2 bytes para xValue
    yValue = packet[2] << 8 | packet[3];  // 2 bytes para yValue
    aState = packet[4];  // 1 byte para el estado de A
    bState = packet[5];  // 1 byte para el estado de B
  }
}

function draw() {
  background(220);
  
  // Visualización de los valores recibidos
  fill(0);
  textSize(16);
  text(`X: ${xValue}`, 10, 30);
  text(`Y: ${yValue}`, 10, 60);
  text(`A: ${aState}`, 10, 90);
  text(`B: ${bState}`, 10, 120);
  
  // Opcional: Puedes agregar gráficos interactivos para los valores
  ellipse(xValue / 10 + width / 2, yValue / 10 + height / 2, 50, 50);
}
```

### Explicación del código:
1. **Serial Connection**: Establecemos una conexión serial entre p5.js y el micro:bit para recibir los datos enviados.
2. **Procesamiento del Paquete Binario**: Cuando se recibe un paquete (que comienza con el valor `0xAA`), extraemos los valores correspondientes a `xValue`, `yValue`, `aState` y `bState` a partir de los bytes del paquete.
3. **Visualización**: Los valores extraídos se muestran en el canvas de p5.js. En este caso, se muestra como texto.

