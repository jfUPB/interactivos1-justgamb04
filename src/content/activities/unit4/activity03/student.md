**Código del micro:bit:**

```python
from microbit import *

uart.init(115200)
display.set_pixel(0,0,9)

while True:
    xValue = accelerometer.get_x()
    yValue = accelerometer.get_y()
    aState = button_a.is_pressed()
    bState = button_b.is_pressed()
    data = "{},{},{},{}\n".format(xValue, yValue, aState, bState)
    uart.write(data)
    sleep(100)  # Envía datos a 10 Hz
```

**¿Qué información se está enviando?**  
Se están enviando cuatro datos por el puerto serial:

1. `xValue`: el valor del eje X del acelerómetro.
2. `yValue`: el valor del eje Y del acelerómetro.
3. `aState`: el estado actual del botón A (True si está presionado, False si no).
4. `bState`: el estado actual del botón B (True si está presionado, False si no).

**¿Cómo se está enviando? ¿Qué significa esta parte del código?**

```python
"{},{},{},{}\n".format(xValue, yValue, aState, bState)
```

Esta línea convierte los datos en una cadena de texto separada por comas, con un salto de línea al final. Por ejemplo, si `xValue = 500`, `yValue = -300`, `aState = True`, `bState = False`, el resultado sería:

```
500,-300,True,False\n
```

Esto permite que el receptor (como un sketch en p5.js) pueda identificar y separar los valores fácilmente.

**¿Cómo se ven los datos en la aplicación SerialTerminal? ¿Qué puedes inferir de la estructura de los datos?**

En SerialTerminal, los datos se ven línea por línea, con los valores separados por comas. Cada línea representa un "paquete" de datos enviado cada 100 ms. La estructura facilita la lectura automática en p5.js, ya que se puede hacer `split()` por comas para extraer los valores.

**¿Por qué se separan los datos con comas y se termina con un salto de línea?**

- **Comas:** permiten dividir fácilmente los datos al recibirlos, por ejemplo, usando `.split(',')`.
- **Salto de línea `\n`:** indica el final de un paquete de datos, útil para saber cuándo un conjunto de datos ha terminado y se puede procesar.

**¿Qué pasaría si no se usaran comas ni salto de línea?**

Sin comas, los datos quedarían todos pegados, como:  
`500-300TrueFalse`, lo que haría muy difícil separar e interpretar cada valor.

Sin salto de línea, no habría forma clara de saber dónde termina un paquete de datos, lo que generaría errores en la interpretación al recibirlos.

**¿Para qué se usa `sleep(100)`? ¿Qué pasaría si no se usara?**

Se usa para pausar el programa 100 milisegundos antes de enviar otro paquete de datos. Esto evita saturar el puerto serial con demasiada información.  
Si no se usara, el micro:bit enviaría datos constantemente, demasiado rápido, lo que podría causar errores de lectura o sobrecarga en el sketch receptor.

**¿Cómo cambian los valores de `xValue` y `yValue` al inclinar el micro:bit?**

- **Inclinar hacia la izquierda:** `xValue` se hace negativo (por ejemplo, -900).
- **Inclinar hacia la derecha:** `xValue` se hace positivo (por ejemplo, 900).
- **Inclinar hacia adelante:** `yValue` se hace positivo.
- **Inclinar hacia atrás:** `yValue` se hace negativo.

Los valores pueden ir aproximadamente entre -1024 y +1024.

**¿Qué valores toman `aState` y `bState` cuando se presionan los botones?**

- Cuando el botón A está presionado: `aState = True`.
- Cuando no está presionado: `aState = False`.

Lo mismo aplica para el botón B (`bState`).

**¿Qué ocurre si en vez de `is_pressed()` se usa `was_pressed()`?**

- `is_pressed()`: devuelve `True` solo mientras el botón está siendo presionado.
- `was_pressed()`: devuelve `True` una sola vez justo después de que el botón fue presionado, y luego vuelve a `False`.

Esto significa que `was_pressed()` detecta "un toque", mientras que `is_pressed()` detecta si el botón está presionado en ese momento exacto.

**Análisis de envío de datos con ejemplo específico:**

Si el micro:bit tiene estos datos:

- `xValue = 969`
- `yValue = 652`
- `aState = True`
- `bState = False`

La cadena enviada sería:

```
969,652,True,False\n
```

**Bytes enviados en HEX:**  
Para comprobarlo, convertimos cada carácter a su código ASCII:

```
969 → 57 54 57  
, → 2C  
652 → 54 53 50  
, → 2C  
True → 54 72 75 65  
, → 2C  
False → 46 61 6C 73 65  
\n → 0A
```

**Bytes enviados (hexadecimal):**

```
39 36 39 2C 36 35 32 2C 54 72 75 65 2C 46 61 6C 73 65 0A
```



