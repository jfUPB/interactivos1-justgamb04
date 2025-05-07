### Propósito Principal

* **`mobile/sketch.js`** actúa como **cliente emisor**. Su función principal es **capturar el movimiento táctil del usuario en un dispositivo móvil** y **enviar esa información al servidor** mediante **eventos Socket.IO**, utilizando un umbral (`threshold`) para optimizar la frecuencia del envío de datos.

* **`desktop/sketch.js`** es el **cliente receptor**. Su rol es **escuchar los datos enviados desde el móvil a través del servidor**, procesarlos y **reflejar visualmente esa información** en el navegador de escritorio mediante un círculo que se mueve en pantalla.

### Análisis del Cliente Móvil

#### Explicación de `touchMoved` y uso de `threshold` y `JSON.stringify`

* `touchMoved()` es una función especial de **p5.js** que se ejecuta cada vez que el usuario **arrastra el dedo** por la pantalla.

* Se compara la posición actual del toque (`mouseX`, `mouseY`) con la última enviada (`lastTouchX`, `lastTouchY`).

* Si el movimiento supera un **umbral mínimo (`threshold = 5`)** o es el primer toque, se genera un objeto con los datos:

  ```javascript
  let touchData = {
    type: 'touch',
    x: mouseX,
    y: mouseY
  };
  ```

* Este objeto se convierte a una cadena de texto en formato **JSON** usando `JSON.stringify(touchData)` y se envía al servidor con `socket.emit('message', ...)`.

* El uso de JSON permite enviar datos estructurados que pueden ampliarse fácilmente (por ejemplo, se podrían añadir más tipos de eventos).

#### Reflexión il

1. **¿Por qué usar un objeto JSON en lugar de una cadena como `"mouseX,mouseY"`?**

   Usar un objeto JSON permite una mejor **estructura**, **extensibilidad** y **legibilidad** de los datos. Se pueden incluir múltiples propiedades (tipo de evento, ID, timestamp, etc.) sin ambigüedad ni necesidad de dividir cadenas manualmente.

2. **¿Qué pasa si se quita el threshold?**

   Se enviarían muchos mensajes por cada mínimo movimiento del dedo. Esto **saturaría la red**, incrementaría el **uso de recursos** y podría causar **lag** o retrasos innecesarios, especialmente en conexiones lentas.

3. **¿Cómo adaptarías el código para que también responda al ratón (por ejemplo, para pruebas en computador)?**

   Se puede agregar un `mouseMoved()` que llame a la misma lógica que `touchMoved()`:

   ```javascript
   function mouseMoved() {
       return touchMoved(); // Reutiliza la lógica de envío
   }
   ```

### Análisis del Cliente de Escritorio

#### Explicación de `socket.on` y procesamiento de datos

* En `setup()`, se conecta al servidor mediante `socket = io();`.
* Se define un **listener** para el evento `'message'`, que recibe la cadena JSON enviada por el cliente móvil.
* Se intenta convertir esa cadena de vuelta a un objeto usando `JSON.parse(data)` dentro de un **bloque `try...catch`**, en caso de que llegue un dato malformado.
* Se verifica si el mensaje es del tipo `'touch'`, y si es así, actualiza las variables `circleX` y `circleY` con las coordenadas recibidas.
* En `draw()`, se dibuja un círculo rojo en esas coordenadas, reflejando en tiempo real la posición del dedo en el móvil.

#### Reflexión 

1. **¿Por qué usar `JSON.parse()` dentro de `try...catch`?**

   Porque `JSON.parse()` lanza un error si el dato recibido **no es una cadena JSON válida**. Si no se maneja con `try...catch`, el script se rompería y detendría la ejecución.

2. **¿Cómo escalar las coordenadas si los canvas tienen tamaños diferentes?**

   Si el canvas móvil mide `300x300` y el de escritorio `600x600`, se puede usar `map()` de p5.js:

   ```javascript
   circleX = map(parsedData.x, 0, 300, 0, 600);
   circleY = map(parsedData.y, 0, 300, 0, 600);
   ```

3. **¿Qué cambiar si el servidor emite un evento diferente, como `'updateDesktop'`?**

   Se debe reemplazar el nombre del evento en el listener:

   ```javascript
   socket.on('updateDesktop', (data) => {
       // misma lógica de procesamiento
   });
   ```
