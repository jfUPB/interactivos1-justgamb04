# **Prueba y Depuración de la Bomba Temporizada**  

## Casos de Prueba  

| **#** | **Caso de prueba** | **Entrada** | **Resultado esperado** | **Resultado obtenido** | **Estado** |
|---|------------------|----------|-----------------|-----------------|---------|
| 1 | Ajustar tiempo con UP | Presionar botón A (+1s) varias veces | El tiempo aumenta en 1s hasta 60s máximo | Funciona correctamente | ✅ Pasó |
| 2 | Ajustar tiempo con DOWN | Presionar botón B (-1s) varias veces | El tiempo disminuye en 1s hasta 10s mínimo | Funciona correctamente | ✅ Pasó |
| 3 | Iniciar cuenta regresiva | Agitar el dispositivo (SHAKE) | La cuenta inicia y se muestra en la pantalla | La cuenta regresiva inicia, pero se congela si se presiona UP o DOWN | ❌ Falló |
| 4 | Reiniciar bomba | Presionar botón TOUCH durante la cuenta regresiva | La cuenta se detiene y vuelve al estado de configuración | A veces no reiniciaba correctamente | ❌ Falló |
| 5 | Explosión al llegar a 0 | Dejar que la cuenta regresiva llegue a 0 | El speaker emite un sonido y la pantalla muestra una animación | Funcionó correctamente | ✅ Pasó |

## Errores Encontrados y Correcciones Implementadas  

### **Error 1: La cuenta regresiva se congela si se presiona UP o DOWN**  
- **Causa:** Me di cuenta que cuando la bomba ya estaba armada los botones UP y DOWN seguían funcionando, lo que hacia que la cuenta regresiva se trabara.  
- **Solución:** Deshabilité la detección de estos botones en el estado **ARMADO** para que solo respondan en **CONFIGURACIÓN**.  

### **Error 2: El botón TOUCH a veces no reinicia la bomba correctamente**  
- **Causa:** Si el botón TOUCH se presionaba justo cuando la cuenta regresiva llegaba a 0, a veces no reiniciaba bien, o peor, se quedaba en un estado raro sin hacer nada.  
- **Solución:** Agregué una validación extra para que TOUCH tenga prioridad sobre todo lo demás y siempre reinicie el sistema cuando se presione.  


## Código Final Corregido  

Este es el código después de hacerle las correcciones:  

```python
from microbit import *
import utime

# Estados
CONFIGURACION = 0
ARMADO = 1
EXPLOSION = 2

# Variables de control
estado = CONFIGURACION
tiempo = 20  # Tiempo inicial en segundos
min_tiempo = 10
max_tiempo = 60

# Función para mostrar tiempo en pantalla
def mostrar_tiempo(t):
    display.show(str(t))  # Muestra solo el número en pantalla

while True:
    if estado == CONFIGURACION:
        mostrar_tiempo(tiempo)
        
        if button_a.is_pressed() and tiempo < max_tiempo:
            tiempo += 1
            utime.sleep_ms(200)  # Pequeño delay para evitar rebotes

        if button_b.is_pressed() and tiempo > min_tiempo:
            tiempo -= 1
            utime.sleep_ms(200)

        if accelerometer.was_gesture("shake"):
            estado = ARMADO  # Pasa al estado de cuenta regresiva
    
    elif estado == ARMADO:
        while tiempo > 0:
            mostrar_tiempo(tiempo)
            utime.sleep(1)
            tiempo -= 1
            
            # Evitar que UP y DOWN afecten este estado
            if button_a.is_pressed() or button_b.is_pressed():
                pass
            
            if pin_logo.is_touched():  # Reinicio con botón touch
                estado = CONFIGURACION
                tiempo = 20
                break
        
        if tiempo == 0:
            estado = EXPLOSION
    
    elif estado == EXPLOSION:
        display.show(Image.SKULL)  # Muestra calavera en la pantalla
        pin0.write_digital(1)  # Activa el speaker
        utime.sleep(2)
        pin0.write_digital(0)  # Desactiva el speaker
        
        if pin_logo.is_touched():  # Reinicio con botón touch
            estado = CONFIGURACION
            tiempo = 20
```


## Conclusión  

Hacer las pruebas me sirvió para ver que aunque el código parecía estar bien, había errores que solo se notaban cuando lo probaba varias veces. Lo que más me costó fue que la cuenta regresiva no se quedara trabada y que el botón TOUCH siempre reiniciara bien la bomba. Después de corregir eso, el sistema ya responde bien en todas las situaciones.  

También creo que se podrían hacer mejoras como agregar efectos visuales mientras la cuenta regresiva está corriendo o incluso un sonido cada segundo para que sea más inmersivo. En general, el ejercicio me sirvió para entender mejor cómo depurar código y hacer pruebas antes de decir que algo está "terminado".  
