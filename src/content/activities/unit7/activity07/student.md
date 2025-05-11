### **Reflexión personal**

**¿Qué concepto o actividad te resultó más fácil de entender o realizar? ¿Por qué crees que fue así?**
La parte que más fácil me resultó fue configurar y utilizar Dev Tunnels, ya que la interfaz de VS Code lo hace bastante intuitivo y ya tenía experiencia compartiendo servidores locales con ngrok o similares. Esto facilitó exponer mi servidor local para pruebas en red con el móvil.

**¿Qué concepto o actividad te presentó mayor dificultad? ¿Qué pasos seguiste para intentar superarla? ¿Qué recursos o estrategias te fueron más útiles?**
La parte más desafiante fue estructurar correctamente la comunicación entre el móvil y el escritorio, especialmente asegurando que los datos de color, posición y `stroke` se transmitieran y se interpretaran correctamente. Para solucionarlo, hice pruebas constantes imprimiendo los datos JSON enviados, revisé la documentación de `Socket.IO`, y comparé con ejemplos previos de las actividades SEEK.

### **Explicación del flujo de información**

Cuando el usuario toca la pantalla del móvil, la aplicación captura la posición del toque (`x`, `y`), el color seleccionado y si se desea que tenga contorno (`stroke`). Estos datos se empaquetan como un objeto y se envían mediante `socket.emit()` al servidor Node.js. Este servidor, gracias a Socket.IO, retransmite esa información al cliente de escritorio en tiempo real, usando `socket.broadcast.emit()`.
En el escritorio, esa información se recibe y se usa para crear una nueva partícula con las propiedades enviadas. Esa partícula se dibuja con p5.js, que la anima y le aplica un comportamiento visual.
VS Code Dev Tunnels hace posible que el móvil pueda acceder al servidor local del computador como si estuviera en Internet.

### **Aplicaciones futuras de lo aprendido**

Creo que lo aprendido en esta unidad tiene mucho potencial para:

* **Experiencias interactivas en exposiciones**: controlar proyecciones o instalaciones usando móviles de los asistentes.
* **Juegos cooperativos locales**: donde varios móviles puedan influir en un mismo entorno visual en pantalla.
* **Prototipos de interfaces IoT**: usando móviles como interfaces temporales para prototipos físicos.
* **Sistemas de votación o feedback en tiempo real**: donde los toques de los usuarios en sus móviles modifiquen visualizaciones compartidas.
