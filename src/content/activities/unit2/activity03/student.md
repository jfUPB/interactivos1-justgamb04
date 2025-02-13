#### Características del micro:bit  
El **BBC micro:bit** es una placa de desarrollo diseñada para la educación en programación y electrónica. Cuenta con un procesador ARM, sensores, botones y conectividad inalámbrica.  

#### **Entradas del micro:bit:**  
1. **Botón A**: Permite detectar cuándo es presionado.  
2. **Botón B**: Similar al botón A, usado para interacción.  
3. **Sensor de luz**: Detecta la intensidad de luz del ambiente.  
4. **Acelerómetro**: Mide movimientos y orientación de la placa.  

#### Salidas del micro:bit:  
##### 1. Matriz de LEDs (5x5):
Permite mostrar texto e imágenes.  
##### 2. Zumbador (en algunas versiones): 
Emite sonidos.  
##### 3. Conexión Bluetooth:
Envía datos a otros dispositivos.  
##### 4. Pines de salida: 
Pueden activar motores o luces externas.  



##### Funciones de MicroPython para Entradas y Salidas 

#### Funciones de entrada:
1. `button_a.is_pressed()`: Detecta si el botón A está presionado.  
   ```python
   from microbit import *
   if button_a.is_pressed():
       display.show("A")
   ```  

2. `accelerometer.get_x()`: Obtiene la aceleración en el eje X.  
   ```python
   from microbit import *
   x = accelerometer.get_x()
   print(x)
   ```  

3. `pin1.read_analog()`: Lee un valor analógico de un pin de entrada.  
   ```python
   from microbit import *
   value = pin1.read_analog()
   print(value)
   ```  

#### Funciones de salida:
1. `display.show()`: Muestra texto o imágenes en la matriz de LEDs.  
   ```python
   from microbit import *
   display.show("H")
   ```  

2. `pin2.write_digital(1)`: Envía un valor digital (1 o 0) a un pin de salida.  
   ```python
   from microbit import *
   pin2.write_digital(1)  # Enciende un LED conectado al pin 2
   ```  

3. `music.play(music.DADADADUM)`: Reproduce sonidos en el micro:bit.  
   ```python
   import music
   music.play(music.DADADADUM)
   ```  

