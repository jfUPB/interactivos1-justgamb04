### Comparación entre protocolo ASCII y protocolo binario:

| **Aspecto**       | **Protocolo ASCII**                                               | **Protocolo Binario**                                               |
|-------------------|-------------------------------------------------------------------|---------------------------------------------------------------------|
| **Eficiencia**    | El protocolo ASCII es menos eficiente porque usa más bytes (cada carácter es de 1 byte), lo que implica mayor uso de recursos de red o memoria para transmitir la misma cantidad de datos. | El protocolo binario es más eficiente ya que puede empaquetar más información en menos bytes (por ejemplo, 2 bytes para un entero de 16 bits). |
| **Velocidad**     | Generalmente más lento debido a la mayor cantidad de caracteres a enviar para representar los datos. | Más rápido, ya que los datos binarios ocupan menos espacio y requieren menos tiempo de procesamiento en la transmisión. |
| **Facilidad**     | Más fácil de implementar y entender por los humanos, ya que se utiliza texto legible. | Más complejo de implementar, ya que es necesario empaquetar y desempaquetar los datos, además de que no es legible por humanos. |
| **Uso de recursos** | Mayor uso de recursos de almacenamiento y transmisión debido a la representación de los datos en texto. | Menor uso de recursos, ya que se reduce el tamaño de los datos que se transmiten. |

**Ejemplo concreto de la aplicación modificada**:  
- En el protocolo **ASCII**, cada valor de `xValue`, `yValue`, y los estados de los botones `aState` y `bState` podrían ser representados por caracteres (por ejemplo, números convertidos a cadenas de texto), lo que requeriría más bytes por cada valor.
- En el protocolo **binario**, los valores se empaquetan eficientemente usando `struct.pack()` para los enteros y los estados, y se agregan en un solo paquete, optimizando el uso de la memoria y la velocidad de transmisión.

### ¿Por qué fue necesario introducir framing en el protocolo binario?

**Framing** es necesario para identificar los límites de un paquete de datos. Sin un marcador claro que delimite el inicio y el final de un paquete, sería imposible saber dónde terminan los datos y comienza el siguiente paquete. En el protocolo binario, los datos se envían en forma de bloques o "paquetes", y el framing ayuda a identificar estos bloques y evitar confusión en la interpretación de los datos.

### ¿Cómo funciona el framing?

El framing en protocolos binarios generalmente se implementa con un **carácter especial** o un **encabezado (header)** que marca el inicio del paquete. Por ejemplo, en el código del micro:bit, el valor `0xAA` sirve como encabezado del paquete, indicando que los datos que siguen a este valor deben ser procesados como un único paquete.

### ¿Qué es un carácter de sincronización?

Un **carácter de sincronización** es un valor específico utilizado en los protocolos de comunicación para indicar que la transmisión de datos está comenzando o que se debe prestar atención a los datos que siguen. En este caso, `0xAA` es el carácter de sincronización en el protocolo binario, señalando el inicio del paquete.

### ¿Qué es el checksum y para qué sirve?

El **checksum** es una suma de verificación que se utiliza para detectar errores en los datos transmitidos. En el código del micro:bit, el checksum se calcula sumando todos los bytes del paquete y tomando el resultado módulo 256. Cuando los datos llegan a la aplicación de p5.js, se recalcula el checksum y se compara con el checksum recibido para verificar que no se hayan producido errores durante la transmisión.

### En la función readSerialData() del programa en p5.js:  
**¿Qué hace la función concat? ¿Por qué?**

La función `concat()` en JavaScript se utiliza para **agregar nuevos datos al final de un array o buffer**. En este caso, se utiliza para concatenar los nuevos datos (`newData`) que se leen del puerto serial al buffer (`serialBuffer`). Esto permite que los datos recibidos se vayan almacenando en el buffer mientras la función sigue leyendo datos del puerto.

### ¿Por qué tenemos un bucle que recorre el buffer solo si este tiene 8 o más bytes?

El bucle recorre el buffer solo si tiene 8 o más bytes porque el paquete de datos completo enviado desde el micro:bit debe tener al menos 8 bytes para ser procesado correctamente. Esto incluye los datos del encabezado, los valores de `xValue`, `yValue`, los estados de los botones, y el checksum. Si el buffer no tiene suficientes bytes, el código no puede procesar un paquete completo y debe esperar a que lleguen más bytes.

### ¿Qué significa `0xaa` en el código?

`0xAA` es el valor hexadecimal que **actúa como encabezado (header)** del paquete de datos. Este valor sirve para **sincronizar** la recepción de datos y para indicar que los bytes que siguen pertenecen a un paquete válido.

### ¿Qué hace la función `shift` y la instrucción `continue`? ¿Por qué?

- **`shift()`** elimina el primer elemento de un array. En este caso, se usa para descartar los bytes que no pertenecen a un paquete válido (es decir, si no comienzan con `0xAA`).
- **`continue`** salta al siguiente ciclo del bucle, sin procesar el paquete actual, si el encabezado no es válido. Esto permite que el código continúe buscando paquetes válidos.

### ¿Qué hace la instrucción `break` si hay menos de 8 bytes?

La instrucción `break` sale del bucle `while` si hay menos de 8 bytes en el buffer, ya que no se puede procesar un paquete incompleto. Esto evita que el programa intente procesar un paquete incompleto que podría provocar errores.

### ¿Cuál es la diferencia entre `slice` y `splice`? ¿Por qué se usa `splice` después de `slice`?

- **`slice()`** crea una nueva copia de una parte de un array sin modificar el array original.
- **`splice()`** elimina o reemplaza elementos en el array original.

Se usa `splice()` después de `slice()` para **eliminar los 8 bytes procesados** del buffer original una vez que ya han sido extraídos y procesados.

### ¿Cómo opera la función `reduce`?

La función `reduce()` en programación funcional **reduce un array a un solo valor** al aplicar una función acumuladora. En este caso, se usa para calcular la suma de los bytes de datos, y el resultado se toma módulo 256 para obtener el checksum.

```javascript
let computedChecksum = dataBytes.reduce((acc, val) => acc + val, 0) % 256;
```

Esta operación suma todos los valores en `dataBytes` y luego toma el residuo de esa suma dividido entre 256.

### ¿Por qué se compara el checksum enviado con el calculado? ¿Para qué sirve esto?

Se compara el checksum recibido con el checksum calculado para verificar que los datos no hayan sido alterados o corrompidos durante la transmisión. Si los checksums no coinciden, significa que ocurrió un error en la transmisión.


### ¿Qué hace la instrucción `continue` en el código?

La instrucción `continue` hace que el bucle continúe con la siguiente iteración sin procesar el paquete actual, si se detecta un error en el checksum. Esto permite que el programa ignore paquetes corruptos y siga buscando paquetes válidos.

### ¿Qué es un DataView? ¿Para qué se usa?

Un **DataView** es un objeto en JavaScript que permite leer y escribir datos en un buffer de manera eficiente, interpretando los bytes en diferentes formatos (por ejemplo, enteros de 16 bits, flotantes, etc.). Se usa para acceder a los datos binarios de manera estructurada y convertirlos en valores útiles.

```javascript
let buffer = new Uint8Array(dataBytes).buffer;
let view = new DataView(buffer);
```

### ¿Por qué es necesario hacer estas conversiones y no simplemente tomar los datos del buffer?

Las conversiones con **DataView** son necesarias porque el buffer de datos es una secuencia de bytes crudos, y para interpretar esos bytes correctamente (por ejemplo, como enteros de 16 bits o valores booleanos), necesitamos usar métodos específicos que conviertan los bytes a los tipos de datos apropiados.
