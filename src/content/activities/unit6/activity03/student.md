## **Dependencias**

**Reflexión:**  
Usar módulos nos ahorra mucho tiempo y esfuerzo. En lugar de programar desde cero cómo servir archivos o cómo gestionar conexiones en tiempo real, usamos librerías como `express` o `socket.io` que ya están optimizadas, documentadas y comprobadas por miles de desarrolladores. Esto mejora la **eficiencia, mantenimiento y seguridad** del código.

## **Creación del Servidor y Socket.IO**

**Reflexión:**  
Estas líneas crean el servidor que escucha las conexiones entrantes y le permiten comunicarse en tiempo real con los navegadores. `socketIO(server)` es clave para habilitar esta comunicación bidireccional.

## **Variables para guardar el estado**

**Reflexión:**  
Las variables `page1` y `page2` guardan los datos actualizados que envía cada cliente. Así, el servidor tiene una "memoria" del estado y puede compartirla con los demás clientes.

## **Sirviendo los archivos del cliente**

**Experimento:**  
Cambié la línea a `app.use(express.static(path.join(__dirname, 'archivos_cliente')));`  
Resultado: al abrir `http://localhost:3000/page1.html` me aparece un error 404 (archivo no encontrado). En la consola del navegador dice que no puede cargar el archivo.  
**Conclusión:** El servidor solo sirve archivos desde la carpeta indicada, y si no existe o no contiene los archivos solicitados, no puede mostrarlos. Volví a dejar la línea como `'views'` y todo funcionó correctamente.

## **Rutas: ¿Qué enviar cuando se pide una URL?**

**Experimento:**  
Cambié `/page1` por `/pagina_uno`.  
- `http://localhost:3000/page1` → no funcionó.  
- `http://localhost:3000/pagina_uno` → sí funcionó.  

**Reflexión:**  
Las rutas son como direcciones exactas. Si las cambias en el servidor, también hay que cambiarlas en el cliente. El servidor solo responde a las rutas que se han definido explícitamente.

## **Socket.IO la Conexión**

**Experimento:**  
Abrí `page1` y luego `page2` en el navegador. En la terminal aparecieron:

```
A user connected - ID: xxxxxxxx1
A user connected - ID: xxxxxxxx2
```

Luego, al cerrar las pestañas, apareció:

```
User disconnected - ID: xxxxxxxx1
User disconnected - ID: xxxxxxxx2
```

**Reflexión:**  
Cada conexión es única. Socket.IO genera un ID que permite identificar cada cliente conectado. Así, el servidor puede saber quién entra y quién se va.

## **Escuchando Mensajes de los Clientes**

**Experimento 1:**  
Al mover la ventana de `page1`, la terminal muestra:

```
Received win1update from ID: xxxxxxxx1 Data: {x:..., y:..., width:..., height:...}
```

Al mover la de `page2`:

```
Received win2update from ID: xxxxxxxx2 Data: {x:..., y:..., width:..., height:...}
```

**Experimento 2:**  
Reemplacé `socket.broadcast.emit('getdata', page1)` por `socket.emit('getdata', page1)`.

Resultado: al mover `page1`, **no se actualiza la visualización en `page2`**.

**Reflexión:**  
`broadcast.emit` envía a todos los demás clientes **excepto** el que envió el mensaje. `socket.emit` se lo envía solo a sí mismo. Por eso la sincronización en tiempo real se rompe si quitamos `broadcast`.

## **Poner en marcha el Servidor**

**Experimento:**  
Cambié `const port = 3000;` por `const port = 3001;`

La terminal mostró:  
```
Server is listening on http://localhost:3001
```

- `http://localhost:3000/page1` → no funcionó.  
- `http://localhost:3001/page1` → funcionó correctamente.  

**Reflexión:**  
El puerto es como la puerta por la que se comunica el servidor. Si cambia el número, la URL también debe cambiar. La función `listen()` pone al servidor a escuchar en ese número específico.

**Resumen:**  
La estructura de `server.js` permite:
- Recibir conexiones de los clientes.
- Escuchar los datos que envían.
- Actualizar el estado global.
- Retransmitir a otros clientes.

Gracias a `socket.io`, todo esto ocurre **en tiempo real**, lo que hace posible experiencias interactivas sincronizadas como el ejemplo de las ventanas compartidas.
