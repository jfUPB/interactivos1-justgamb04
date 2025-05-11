## **Diagrama**
![image](https://github.com/user-attachments/assets/a982a0ad-ca33-48b1-b966-f0db2929ca09)

### **Pasos para crear el diagrama:**

1. **Componentes clave:**

   * **Aplicación cliente móvil**: Esta es la aplicación en el navegador del móvil que usa `mobile/sketch.js`. En esta parte, el usuario interactúa tocando la pantalla, eligiendo el color de la partícula y si la partícula tendrá un contorno (stroke).
   * **Servidor Node.js**: Este servidor utiliza `server.js` junto con Express y Socket.IO para manejar la comunicación en tiempo real entre el cliente móvil y el cliente de escritorio.
   * **Servicio de VS Code Dev Tunnels**: Este servicio actúa como un puente entre tu servidor local y el internet para que puedas acceder a tu aplicación desde diferentes dispositivos.
   * **Aplicación cliente de escritorio**: Esta es la aplicación que corre en el navegador del escritorio y usa `desktop/sketch.js`. Aquí es donde se dibujan las partículas según la información recibida del móvil.
   * **El usuario**: Interactúa con la aplicación móvil tocando la pantalla para generar las partículas.

2. **Flujo de información:**

   * El usuario toca la pantalla en el móvil, lo que genera un **evento touch** con coordenadas `(x, y)`, un **color** aleatorio y si se aplica **stroke**.
   * Esta información se **emite** desde el móvil a través de **Socket.IO** usando `socket.emit('message', JSON)` al servidor.
   * El servidor recibe la información y la **reenvía** a todos los clientes conectados mediante `socket.broadcast.emit('message', JSON)`.
   * La aplicación cliente de escritorio recibe la información a través de **WebSocket** y dibuja la partícula en las coordenadas `(x, y)` con el color y el estilo de `stroke` especificados.

3. **El servicio de Dev Tunnels**: Entre el servidor local y el internet, se utiliza un **túnel** para facilitar la comunicación entre los diferentes dispositivos, de manera que puedas ver la aplicación funcionando tanto en el móvil como en el escritorio, aunque estén en diferentes redes.


1. **Componentes**:

   * **Caja 1**: **Aplicación móvil** (usuario interactuando con el móvil).
   * **Caja 2**: **Servidor Node.js** (con Socket.IO y Express).
   * **Caja 3**: **Servicio de VS Code Dev Tunnels** (puente entre internet y el servidor local).
   * **Caja 4**: **Aplicación de escritorio** (donde se dibujan las partículas).

2. **Flujo de información**:

   * Desde la **Aplicación móvil** al **Servidor**: "Evento touch (x, y), color, stroke".
   * Desde el **Servidor** a la **Aplicación de escritorio**: "Petición WebSocket (coordenadas, color, stroke)".
   * Entre el **Servidor** y el **Servicio Dev Tunnels**: "Petición HTTP/WebSocket".
   * Desde la **Aplicación de escritorio**: "Dibujo en canvas" con las coordenadas, color y stroke.

3. **Explicaciones** debajo del diagrama:

   * **Aplicación móvil**: El usuario interactúa tocando la pantalla. El móvil envía las coordenadas del toque, el color y si se aplica `stroke`.
   * **Servidor Node.js**: Actúa como intermediario para recibir los datos del móvil y enviarlos a la aplicación de escritorio mediante WebSockets.
   * **Servicio de VS Code Dev Tunnels**: Permite que la comunicación entre el servidor y los dispositivos conectados sea posible desde cualquier lugar de la red, sin importar si están en redes diferentes.
   * **Aplicación de escritorio**: Recibe los datos a través de WebSockets, los interpreta y actualiza la visualización de las partículas en el canvas del navegador.


1. **Caja 1: Aplicación móvil**

   * El usuario interactúa tocando la pantalla.
   * El móvil envía:

     * Coordenadas `(x, y)`
     * Color
     * `stroke` (booleano)

2. **Caja 2: Servidor Node.js**

   * Recibe la información del móvil.
   * Reenvía los datos a todos los clientes de escritorio.

3. **Caja 3: Servicio de VS Code Dev Tunnels**

   * Intermediario entre el servidor local y los dispositivos en internet.

4. **Caja 4: Aplicación de escritorio**

   * Recibe los datos desde el servidor.
   * Dibuja las partículas en el canvas.
