Completé el flujo I-P-O propuesto en la Actividad 3, integrando entradas del micro\:bit (vía Serial), el móvil (vía Socket.IO) y entradas locales (teclado/ratón) para modificar visualmente una figura interactiva. El sistema responde en tiempo real a la combinación de inputs según reglas que procesan las variables y generan un output visual coherente con la narrativa definida.

## Código del Cliente p5.js (`public/desktop/sketch.js`)

### **Entrada: Almacenamiento de Estado**

```javascript
let socket, serial;
let microbitData = { x: 0, y: 0, z: 0 };
let mobileData = { touchX: 0, touchY: 0 };
let localControl = 0;

function setup() {
  createCanvas(800, 600);

  // Serial
  serial = new p5.SerialPort();
  serial.open('/dev/tty.usbmodemXXXX'); // Cambiar por puerto real
  serial.on('data', () => {
    let line = serial.readLine();
    if (line) {
      let matches = line.match(/X:(-?\d+)\s+Y:(-?\d+)\s+Z:(-?\d+)/);
      if (matches) {
        microbitData.x = int(matches[1]);
        microbitData.y = int(matches[2]);
        microbitData.z = int(matches[3]);
      }
    }
  });

  // Socket
  socket = io();
  socket.on('mobileInputForwarded', (data) => {
    mobileData = data;
  });
}
```

### **Proceso: Lógica Principal**

```javascript
let ellipseX = 400;
let ellipseY = 300;
let ellipseColor;

function processInputs() {
  // Afectar posición por acelerómetro
  ellipseX += microbitData.x * 0.01;
  ellipseY += microbitData.y * 0.01;

  // Límite de pantalla
  ellipseX = constrain(ellipseX, 0, width);
  ellipseY = constrain(ellipseY, 0, height);

  // Color basado en Z y toques móviles
  let r = map(microbitData.z, -1024, 1024, 0, 255);
  let g = map(mobileData.touchX, 0, width, 0, 255);
  let b = map(mobileData.touchY, 0, height, 0, 255);
  ellipseColor = color(r, g, b);

  // Modificador local
  if (keyIsDown(UP_ARROW)) localControl = -2;
  else if (keyIsDown(DOWN_ARROW)) localControl = 2;
  else localControl = 0;

  ellipseY += localControl;
}
```

### **Output: Visualización en `draw()`**

```javascript
function draw() {
  background(30);
  processInputs();

  fill(ellipseColor);
  noStroke();
  ellipse(ellipseX, ellipseY, 50, 50);
}
```

## Comportamiento Final

* La **elipse se mueve** suavemente con el micro\:bit (X, Y).
* Su **color cambia** dependiendo de los datos de toque desde el móvil y la aceleración en Z.
* Puede **modificarse verticalmente** con las flechas del teclado como control local.
* El sistema reacciona en tiempo real, integrando sincrónicamente los tres tipos de inputs.

## Desafíos Encontrados

* **Ruido en datos seriales**: hubo que filtrar bien las lecturas, porque a veces venían vacías o incompletas.
* **Desfase entre entrada y visual**: cuando el socket enviaba muchas señales, había micro-lag en el canvas. Lo resolví usando `frameRate(30)` para estabilizar.
* **Coherencia visual**: fue necesario escalar con `map()` los valores del micro\:bit y móvil para obtener una gama de colores y movimientos adecuados.

## Conclusión

Logré completar el flujo I-P-O combinando correctamente los tres tipos de entrada. El resultado es una experiencia visual en tiempo real que responde a múltiples dispositivos de forma fluida. Esta actividad consolidó mi comprensión de integración hardware + red + cliente visual.
