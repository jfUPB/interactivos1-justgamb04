### **Análisis del Código con Máquinas de Estados**  

Este código hace que dos píxeles de la pantalla LED del Micro:bit parpadeen de manera independiente usando una máquina de estados. Cada píxel tiene un tiempo de encendido y apagado diferente, y el programa los actualiza constantemente sin detenerse.  

---

## **1. ¿Cómo funciona el programa?**  

1. **Se crea la clase `Pixel`**, que representa un punto en la pantalla LED. Cada píxel tiene:
   - Una **posición** en la matriz (X, Y).
   - Un **estado inicial** (apagado o encendido).
   - Un **intervalo de tiempo**, que define cada cuánto cambia entre encendido y apagado.
   - Un **estado interno**, que controla su comportamiento.  

2. **El método `update()`** controla lo que pasa con cada píxel:
   - Si el píxel está en el estado `"Init"`, se enciende o apaga y empieza a contar el tiempo.
   - Luego pasa al estado `"WaitTimeout"`, donde espera hasta que pase el intervalo de tiempo definido.  
   - Cuando el tiempo se cumple, el píxel cambia de estado (de apagado a encendido o viceversa).  

3. **Se crean dos píxeles con tiempos diferentes**:
   - `pixel1 = Pixel(0,0,0,1000)` → Cambia cada **1 segundo**.  
   - `pixel2 = Pixel(4,4,0,500)` → Cambia cada **0.5 segundos**.  

4. **El `while True` hace que el programa corra en un bucle infinito**, llamando constantemente a `update()` para cada píxel. Así se mantiene el parpadeo sin detener el programa.  

---

## **2. Identificación de Estados, Eventos y Acciones**  

### **Estados**  
Los estados son los momentos en los que el programa está esperando algo. En este código hay dos principales:  

1. **"Init"** → Se configura el píxel y se empieza a contar el tiempo.  
2. **"WaitTimeout"** → Se espera a que pase el tiempo antes de cambiar el estado del píxel.  

### **Eventos**  
Los eventos son las cosas que hacen que el programa cambie de estado. Aquí hay dos principales:  

1. **Cuando inicia el programa** → Se entra en el estado `"Init"`.  
2. **Cuando pasa el tiempo definido (`utime.ticks_diff()`)** → El píxel cambia de estado.  

### **Acciones**  
Las acciones son lo que el programa hace cuando ocurre un evento. En este caso:  

1. **Encender o apagar el píxel (`display.set_pixel()`)** → Se actualiza la pantalla LED.  
2. **Reiniciar el temporizador (`self.startTime = utime.ticks_ms()`)** → Se vuelve a contar el tiempo de espera.
