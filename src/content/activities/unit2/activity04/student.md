
#### Experimento 1: Lectura del Botón A

##### ¿Qué querías comprobar?
Quise comprobar si el Micro:bit detecta correctamente cuando se presiona el botón A y mostrar un mensaje en la pantalla LED.

##### ¿Cuál fue tu hipótesis?
Si presiono el botón A, entonces el Micro:bit mostrará un mensaje en la pantalla LED.

###### Código:
```python
from microbit import *

while True:
    if button_a.is_pressed():
        display.show("A")
    else:
        display.clear()
```

##### Descripción de los resultados:
Al presionar el botón A, la pantalla LED mostró la letra "A" y cuando lo soltaban, la pantalla se apagaba.

##### Análisis de los resultados:
El Micro:bit detectó correctamente la entrada digital del botón A. Esto sucede porque `button_a.is_pressed()` devuelve `True` cuando se presiona el botón y `False` cuando no lo está.

##### Conclusiones:
El Micro:bit puede detectar con precisión el estado del botón A y responder mostrando información en la pantalla LED.


### **Experimento 2: Lectura del Botón B**

**¿Qué querías comprobar?**
Quise comprobar si el botón B funciona de manera similar al botón A y si se puede mostrar un mensaje diferente.

**¿Cuál fue tu hipótesis?**
Si presiono el botón B, entonces la pantalla LED mostrará la letra "B".

**Código:**
```python
from microbit import *

while True:
    if button_b.is_pressed():
        display.show("B")
    else:
        display.clear()
```

##### Descripción de los resultados:
Cuando presioné el botón B, la pantalla LED mostró la letra "B" y desapareció al soltar el botón.

##### Análisis de los resultados:
El comportamiento es similar al del botón A, ya que `button_b.is_pressed()` devuelve `True` cuando se presiona.

##### Conclusiones:
El Micro:bit maneja de la misma manera la lectura de ambos botones físicos, permitiendo realizar acciones diferentes según el botón presionado.


#### Experimento 3: Lectura del Botón Virtual del Logo

##### ¿Qué querías comprobar?
Quise comprobar si el Micro:bit reconoce la entrada táctil del logo como un botón virtual y muestra un icono en la pantalla LED.

#####¿Cuál fue tu hipótesis?
Si toco el logo del Micro:bit, entonces la pantalla LED mostrará un corazón.

###### Código:
```python
from microbit import *

while True:
    if pin_logo.is_touched():
        display.show(Image.HEART)
    else:
        display.clear()
```

##### Descripción de los resultados:
Cuando toqué el logo del Micro:bit, la pantalla LED mostró un corazón. Al soltarlo, la pantalla se apagó.

##### Análisis de los resultados:
El Micro:bit detectó correctamente la capacitancia del logo como una entrada digital. `pin_logo.is_touched()` funciona de manera similar a los botones físicos, pero responde al tacto.

##### Conclusiones:
El logo del Micro:bit funciona como un botón virtual sensible al tacto y puede utilizarse para interacciones adicionales en los programas.



#### Conclusiones Generales
- El Micro:bit detecta correctamente los botones A y B, permitiendo mostrar mensajes diferentes según el botón presionado.
- El logo del Micro:bit también funciona como una entrada digital, permitiendo interacciones táctiles.
- Estos experimentos permiten comprender mejor cómo manejar entradas digitales en Micro:bit para futuros proyectos interactivos.

