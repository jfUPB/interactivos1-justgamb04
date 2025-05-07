### **1. Conexión inicial y errores**

**¿Qué pasó cuando refrescaste con el servidor apagado?**
- Apareció un error en la consola del navegador indicando que no se pudo establecer conexión con el servidor (`WebSocket connection to 'ws://localhost:3000/socket.io/?EIO=4&transport=websocket' failed`). Esto confirma que el cliente está intentando conectarse pero no hay servidor disponible.

**¿Y cuando lo volviste a encender y refrescaste?**
- El error desapareció. El mensaje "Connected to server - My ID: ..." apareció, indicando que la conexión se estableció correctamente y se recibió un ID de cliente.

### **2. Enviar estado inicial al conectarse**

**¿Qué ocurrió cuando comentaste `socket.emit('win2update', ...)` y abriste las dos páginas?**
- En `page1`, no se mostró información sobre `page2` hasta que moví la ventana de `page2`, lo que desencadenó una actualización.

**¿Qué aprendiste?**
- Enviar el estado inicial al conectarse permite que el otro cliente tenga información desde el principio, sin esperar a que ocurra un cambio. Esto mejora la sincronización desde el inicio y evita mostrar datos vacíos o incorrectos.

### **3. Recibiendo datos del servidor**

**¿Qué ocurrió cuando moviste `page1`?**
- En la consola de `page2`, apareció:  
  `Page 2 received data about the other window: {x: ..., y: ..., width: ..., height: ...}`  
  Lo que indica que el servidor está reenviando correctamente los datos emitidos por `page1`.

**¿Y cuando moviste `page2`?**
- No apareció el mensaje anterior, porque el evento `getdata` no es emitido para sí mismo, solo se propaga a la otra ventana. Esto demuestra cómo funciona `broadcast.emit()` en el servidor.

### **4. Detectar cambios y enviar actualizaciones**

**¿Cuándo aparece el log “Page 2 detected change…”?**
- Aparece solo cuando muevo o cambio el tamaño de la ventana `page2`.

**¿Y cuando está quieta?**
- No aparece ningún mensaje, lo que demuestra que el cliente evita enviar datos innecesarios.

**Reflexión: ¿Por qué es eficiente esta estrategia?**
- Comparar el estado actual con el anterior evita enviar datos si nada ha cambiado. Esto reduce el tráfico de red, mejora el rendimiento y evita sobrecargar el servidor con información redundante.

### **5. Visualización creativa**

**Cambios que intenté:**
1. **Color del fondo según la distancia:**
   ```js
   let distancia = resultingVector.mag();
   background(map(distancia, 0, 1000, 255, 0));
   ```
   Resultado: el fondo se oscurece a medida que las ventanas están más lejos.

2. **Tamaño del círculo local depende del ancho de la otra ventana:**
   Modifiqué `drawCircle` para aceptar un tercer parámetro `size`, y usé:
   ```js
   drawCircle(point2[0], point2[1], remotePageData.width / 2);
   ```
   Resultado: el círculo local cambia dinámicamente de tamaño si la otra ventana cambia su tamaño.

3. **Color de la línea depende de la posición relativa:**
   ```js
   if (resultingVector.x < 0) {
       stroke('blue');
   } else {
       stroke('red');
   }
   ```
   Resultado: la línea cambia de color según si `page1` está a la izquierda o a la derecha de `page2`.

**Reflexión final:**
- Me gustó mucho ver cómo se puede construir una forma visual de "sentir" dónde está la otra ventana. La comunicación entre clientes a través del servidor se volvió más tangible. Los cambios visuales hacen más comprensible la abstracción de coordenadas remotas y ayudan a imaginar formas creativas de interacción multi-ventana.
