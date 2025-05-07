### Enlace a la aplicación original (sin modificar)  
[Ejemplo original en p5.js](https://editor.p5js.org/just_gamb04/sketches/PDnMbqTB0)

### Análisis del código original  
El código original dibuja una elipse que sigue la posición del mouse en el canvas, y cambia su color al presionar el clic. No hay uso de estados, ni de sensores externos, solo interacción básica con el mouse.

### Modificaciones para integración con micro:bit

El nuevo comportamiento será:
- La elipse se moverá horizontalmente con base en el valor del eje X del acelerómetro del micro:bit.
- La posición vertical dependerá del valor Y del acelerómetro.
- El color de la elipse cambiará si el botón A o B está presionado en el micro:bit.

#### Código del micro:bit (ya dado)

```python
from microbit import *

uart.init(115200)
display.set_pixel(0,0,9)

while True:
    xValue = accelerometer.get_x()
    yValue = accelerometer.get_y()
    aState = button_a.is_pressed()
    bState = button_b.is_pressed()
    data = "{},{},{},{}\n".format(xValue, yValue, aState,bState)
    uart.write(data)
    sleep(100)
```

### Código de p5.js Modificado

```javascript
let serial;
let x = 0;
let y = 0;
let buttonA = 0;
let buttonB = 0;

function setup() {
  createCanvas(400, 400);
  serial = new p5.SerialPort();

  // Lista de puertos disponibles
  serial.list();
  serial.open('/dev/tty.usbmodem1101'); // Asegúrate de usar el nombre correcto de tu puerto

  serial.on('data', serialEvent);
  background(220);
}

function serialEvent() {
  let data = serial.readLine().trim();
  if (data.length > 0) {
    let values = data.split(',');
    if (values.length === 4) {
      x = map(parseInt(values[0]), -1024, 1024, 0, width);
      y = map(parseInt(values[1]), -1024, 1024, 0, height);
      buttonA = int(values[2]);
      buttonB = int(values[3]);
    }
  }
}

function draw() {
  background(220);
  
  // Si se presiona A o B, cambiar el color de la elipse
  if (buttonA === 1 || buttonB === 1) {
    fill(0, 102, 204);
  } else {
    fill(255, 204, 0);
  }
  
  noStroke();
  ellipse(x, y, 50, 50);
}
```

### Enlace a la aplicación modificada en el editor de p5.js  
[https://editor.p5js.org/just_gamb04/sketches/PDnMbqTB0](https://editor.p5js.org/just_gamb04/sketches/3_f8wd71U)

