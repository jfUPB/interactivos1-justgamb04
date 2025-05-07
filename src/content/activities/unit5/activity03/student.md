
### Bitácora 

#### **¿Por qué antes era necesario delimitar la información y usar un salto de línea, y ahora no?**

En la unidad anterior, el envío de datos se hacía en texto (string), por lo que era necesario delimitar los valores con un separador (como comas) y marcar el final del paquete con un salto de línea (`\n`). Esto era necesario porque los datos no tenían una estructura binaria fija, así que el receptor debía saber cuándo terminaba un paquete y dónde empezaba el siguiente.

Ahora, como el envío se hace en formato **binario** y cada paquete tiene un **tamaño fijo de 6 bytes**, el receptor sabe exactamente cuántos bytes debe leer. Por lo tanto, ya no es necesario un delimitador ni un salto de línea.

#### **Comparación entre el código anterior y el nuevo código de p5.js**

**Cambios clave:**

- Se eliminó el análisis de strings (como `.split(',')`).
- Se reemplazó por lectura binaria directa con `readBytes(6)`.
- Se usan `DataView` y `Uint8Array` para interpretar los bytes.
- Se incorporó una lógica de verificación con *checksum* y *framing* en la versión final.

#### **Observaciones al ejecutar el código sin framing**

Al ejecutar el código varias veces, puede observarse un error de sincronización:

```
microBitX: 500 microBitY: 524 microBitAState: true microBitBState: false
...
microBitX: 3073 microBitY: 1 microBitAState: false microBitBState: false
```

Este error ocurre porque p5.js puede comenzar a leer los datos a la mitad de un paquete, lo que desalineará los bytes y generará interpretaciones incorrectas. Esto se debe a que no hay nada que indique cuándo comienza realmente un paquete.

#### **¿Por qué se necesita framing?**

El framing permite:

- **Sincronización**: con un byte de inicio (por ejemplo, `0xAA`), p5.js puede saber exactamente dónde comienza un paquete.
- **Verificación de integridad**: el checksum asegura que los datos no estén corruptos.
- **Recuperación de errores**: si se pierde algún byte, el receptor puede descartar datos hasta encontrar el próximo paquete válido.

#### **Cambios en el programa del micro:bit (versión final)**

- Se agregó un byte de inicio (`0xAA`).
- Se mantiene el formato binario `>2h2B` (2 enteros, 2 booleanos).
- Se añade un checksum: suma de los datos módulo 256.
- El paquete completo ahora tiene 8 bytes: `1 (header) + 6 (data) + 1 (checksum)`.

#### **Cambios en el programa de p5.js (versión final)**

- Se implementó un **buffer de recepción** (`serialBuffer`) para almacenar datos parciales.
- Se lee cualquier cantidad de bytes disponibles y se buscan headers válidos (`0xAA`).
- Se valida el paquete usando el checksum.
- Se descartan bytes inválidos hasta encontrar un paquete válido.

#### **¿Qué se observa en la consola al usar la versión final con framing y checksum?**

- Los valores se imprimen correctamente y de forma estable.
- No se presentan errores de sincronización como en la versión sin framing.
- Si llega un paquete corrupto (por ruido o pérdida de datos), el sistema lo detecta y lo descarta sin afectar los demás.

Ejemplo de consola:
```
microBitX: 123 microBitY: -45 microBitAState: true microBitBState: false
microBitX: 120 microBitY: -40 microBitAState: false microBitBState: false
```
