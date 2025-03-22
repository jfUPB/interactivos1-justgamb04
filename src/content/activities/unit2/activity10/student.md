#### Explicación del funcionamiento del programa y concurrencia  

Este programa implementa una máquina de estados para gestionar una secuencia de imágenes en la pantalla LED del micro:bit y, al mismo tiempo, permite que el usuario interrumpa la secuencia presionando el botón A. Esto significa que el sistema puede atender varios eventos de manera **concurrente**, ya que el flujo de ejecución no se detiene mientras espera la entrada del usuario.  

El programa tiene cuatro estados principales:  

1. **STATE_HAPPY**: Se muestra una cara feliz durante 1500 ms.  
2. **STATE_SMILE**: Se muestra una cara sonriente durante 1000 ms.  
3. **STATE_SAD**: Se muestra una cara triste durante 2000 ms.  
4. **STATE_INIT**: Estado inicial que simplemente configura el sistema y pasa a `STATE_HAPPY`.  

Cada estado tiene una **transición automática** basada en el tiempo transcurrido y una **transición manual** si el usuario presiona el botón A.  

- Si el usuario **no presiona el botón A**, las expresiones cambian automáticamente siguiendo la secuencia: `HAPPY → SMILE → SAD → HAPPY`.  
- Si el usuario **presiona el botón A**, el sistema interrumpe la secuencia y cambia inmediatamente de estado según la siguiente lógica:  
  - Si está en `STATE_HAPPY`, cambia a `STATE_SAD`.  
  - Si está en `STATE_SMILE`, cambia a `STATE_HAPPY`.  
  - Si está en `STATE_SAD`, cambia a `STATE_SMILE`.  

El uso de una máquina de estados permite estructurar el programa de manera clara y organizada, asegurando que las transiciones sean predecibles y que se puedan manejar múltiples eventos sin que el programa se "congele" o tenga comportamientos inesperados.  

#### Vectores de prueba  

Para validar que el sistema funciona correctamente, se han definido los siguientes vectores de prueba:  

##### Prueba 1: Validar la transición automática entre estados  
- **Condición inicial:** El micro:bit está en `STATE_HAPPY`.  
- **Evento generado:** Se deja correr el programa sin presionar ningún botón.  
- **Resultado esperado:** Después de 1500 ms, la pantalla debe cambiar a `STATE_SMILE`, luego de 1000 ms debe cambiar a `STATE_SAD`, y después de 2000 ms debe volver a `STATE_HAPPY`.  
- **Resultado obtenido:** Se observa si el cambio ocurre correctamente sin intervención del usuario.  
- **¿Prueba pasada?** Sí, si la secuencia se repite correctamente.  

##### Prueba 2: Interrumpir la secuencia con el botón A  
- **Condición inicial:** El micro:bit está en `STATE_SMILE`.  
- **Evento generado:** Se presiona el botón A.  
- **Resultado esperado:** La pantalla debe cambiar inmediatamente a `STATE_HAPPY`, sin esperar a que pasen los 1000 ms del `STATE_SMILE`.  
- **Resultado obtenido:** Se prueba físicamente presionando el botón y observando si el cambio ocurre de inmediato.  
- **¿Prueba pasada?** Sí, si la pantalla cambia sin esperar el tiempo predeterminado.  

##### Prueba 3: Validar que el botón A funciona en todos los estados  
- **Condición inicial:** Se deja que el sistema pase por todos los estados (`STATE_HAPPY → STATE_SMILE → STATE_SAD`).  
- **Evento generado:** En cada estado, se presiona el botón A y se observa el cambio inmediato.  
- **Resultado esperado:**  
  - Si se presiona en `STATE_HAPPY`, cambia a `STATE_SAD`.  
  - Si se presiona en `STATE_SMILE`, cambia a `STATE_HAPPY`.  
  - Si se presiona en `STATE_SAD`, cambia a `STATE_SMILE`.  
- **Resultado obtenido:** Se verifica si el sistema reacciona correctamente en cada caso.  
- **¿Prueba pasada?** Sí, si en cada estado el cambio se da de inmediato según lo esperado.

  
Con estos vectores de prueba, podemos asegurar que el programa funciona correctamente, manejando la concurrencia entre la secuencia de imágenes y la interacción con el usuario. La implementación de la máquina de estados hace que el sistema sea estable, predecible y fácil de depurar en caso de errores.
