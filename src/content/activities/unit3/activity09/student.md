## **Vectores de Prueba**

| **Estado Inicial** | **Evento Recibido** | **Acción Esperada** | **Nuevo Estado** | **Resultado** |
|--------------------|--------------------|---------------------|------------------|---------------|
| INACTIVA | Presionar **botón A** (micro:bit) | La bomba se activa y comienza la cuenta regresiva | ARMED | [X] |
| INACTIVA | Enviar **"A"** desde p5.js | La bomba se activa y comienza la cuenta regresiva | ARMED | [X] |
| ARMED | Presionar **botón B** (micro:bit) | La bomba se desactiva | INACTIVA | [X] |
| ARMED | Enviar **"B"** desde p5.js | La bomba se desactiva | INACTIVA | [X] |
| ARMED | Agitar micro:bit (**Shake S**) | Disminuye el tiempo | ARMED (con menos tiempo) | [X] |
| ARMED | Enviar **"S"** desde p5.js | Disminuye el tiempo | ARMED (con menos tiempo) | [X] |
| ARMED | El tiempo llega a **0** | La bomba explota y se reinicia | EXPLODED → INACTIVA | [X] |
| EXPLODED | Presionar **botón A** (micro:bit) | No debería activarse directamente, necesita reset | EXPLODED | [X] |
| EXPLODED | Enviar **"A"** desde p5.js | No debería activarse directamente, necesita reset | EXPLODED | [X] |
| EXPLODED | Presionar **botón touch T** (micro:bit) | Reinicia la bomba | INACTIVA | [X] |
| EXPLODED | Enviar **"RESET"** desde p5.js | Reinicia la bomba | INACTIVA | [X] |
| CONFIG | Presionar **botón B** (micro:bit) | Disminuye el tiempo inicial | CONFIG (nuevo tiempo) | [X] |
| CONFIG | Enviar **"B"** desde p5.js | Disminuye el tiempo inicial | CONFIG (nuevo tiempo) | [X] |


## **Pruebas de Regresión**
**Errores encontrados y correcciones aplicadas:**  

1. **Error:**  
   - Cuando se enviaba `"A"` desde p5.js, la bomba **no cambiaba de estado** a ARMED.  
   - **Causa:** No se estaba procesando correctamente el mensaje en `serialEvent()`.  
   - **Corrección:** Se agregó `.trim()` para eliminar espacios en blanco o saltos de línea.  

2. **Error:**  
   - Cuando la bomba llegaba a `EXPLODED`, al presionar **botón A** podía volver a activarse sin resetear.  
   - **Causa:** No había una condición que restringiera la activación desde `EXPLODED`.  
   - **Corrección:** Se agregó una validación en `sketch.js` para solo permitir reset en `EXPLODED`.  

3. **Error:**  
   - Al enviar `"S"` desde p5.js cuando el tiempo ya era 0, la bomba **se reiniciaba sin explotar**.  
   - **Causa:** No había una validación en `serialEvent()` para manejar este caso.  
   - **Corrección:** Se agregó una condición para verificar que si el tiempo llega a 0, la bomba explota antes de reiniciarse.  
