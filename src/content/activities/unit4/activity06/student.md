### ¿Qué es un protocolo de comunicación y por qué es importante en la comunicación serial?

Un **protocolo de comunicación** es un conjunto de reglas que permite que dos dispositivos puedan intercambiar datos de manera clara, estructurada y entendible. En comunicación serial, es importante porque permite que tanto el micro:bit como p5.js se entiendan sin confusión al enviar y recibir datos.

> Fragmento de código (micro:bit - Actividad 05):
```python
data = "{},{},{},{}\n".format(xValue, yValue, aState, bState)
uart.write(data)
```
Este protocolo define que los datos se mandan separados por comas y finalizados con `\n`.

### ¿Por qué se separan los datos con comas en el protocolo ASCII que exploramos?

Porque las comas permiten **distinguir cada dato** enviado en una sola cadena de texto. Así, en p5.js podemos usar `.split(",")` para separarlos fácilmente en variables distintas.

> Fragmento de código (p5.js):
```javascript
let values = data.trim().split(",");
let x = int(values[0]);
let y = int(values[1]);
```
### ¿Por qué es necesario terminar los datos con un carácter que marque el fin del mensaje?

Porque en comunicación serial, el receptor no sabe cuándo termina un mensaje a menos que haya un delimitador claro, como `\n`. Así p5.js sabe que debe leer hasta ese punto exacto.

> Fragmento de código (p5.js):
```javascript
if (port.availableBytes() > 0) {
  let data = port.readUntil("\n");
}
```

### ¿Por qué fue necesario usar una máquina de estados en la aplicación modificada de p5.js?

Porque permite **organizar la lógica de la aplicación** en distintas etapas o comportamientos controlados, según lo que ocurre en el micro:bit. Por ejemplo, moverse o cambiar de color según qué botón se presione.

> Fragmento de código (p5.js):
```javascript
if (buttonA) {
  estado = "rojo";
} else if (buttonB) {
  estado = "azul";
}
```
### ¿Cómo se formatean los datos en el micro:bit para ser enviados por el puerto serial?

Se convierten a texto en formato ASCII, se separan con comas y se terminan con un salto de línea.

> Fragmento de código:
```python
data = "{},{},{},{}\n".format(xValue, yValue, aState, bState)
```

### ¿Qué significa que los datos enviados por el micro:bit están codificados en ASCII?

Que cada carácter enviado por el puerto serial representa un valor en la tabla ASCII, lo que permite que p5.js los lea como texto (string), antes de convertirlos a número.

### ¿Por qué es necesario en la aplicación de p5.js preguntar si hay bytes disponibles en el puerto serial antes de leerlos?

Porque si no hay datos disponibles, y tratamos de leer, puede lanzar un error o leer información incompleta.

> Fragmento:
```javascript
if (port.availableBytes() > 0) {
  let data = port.readUntil("\n");
}
```
### ¿Qué pasa si esto no se hace?

Puede que se intente leer un dato cuando no hay nada disponible, resultando en `undefined`, errores de conversión o comportamiento inesperado.

### ¿Cómo se elimina el retorno de carro o salto de línea de un string en p5.js?

Usando la función `.trim()` que elimina espacios, `\r` o `\n` al inicio o al final del string.

> Fragmento:
```javascript
let data = port.readUntil("\n");
data = data.trim();
```
### Si una cadena tiene información separada por espacios y quieres dividir dicha información en varias cadenas individuales ¿Qué función de p5.js usarías?

Usaría `.split(" ")`.

> Ejemplo:
```javascript
let valores = "123 456 789".split(" ");
```
### ¿Por qué es necesario en la aplicación del caso de estudio convertir las cadenas a números enteros antes de usarlas en el sketch de p5.js?

Porque al recibir los datos como texto, no se pueden usar directamente en operaciones matemáticas o de dibujo (por ejemplo, para mover una figura).

> Fragmento:
```javascript
let x = int(values[0]);  // Convertimos de string a número entero
```

### Si el micro:bit tiene los siguientes datos  
**xValue: 123, yValue: 756, aState: False, bState: True**  
¿Qué bytes se enviarían por el puerto serial?

Se enviaría la siguiente cadena ASCII:
```
123,756,False,True\n
```
Cada carácter es un byte codificado en ASCII.

### ¿Qué aprendiste nuevo del micro:bit que no sabías antes?

Aprendí que el micro:bit puede enviar datos a través del puerto serial usando UART, y que puede capturar aceleración, botones y más, lo cual permite comunicar sensores con otras plataformas.
