### ¿Por qué es necesario Dev Tunnels?

Dev Tunnels es necesario porque el servidor que se ejecuta en mi computador escucha en `localhost:3000`, una dirección que solo es válida dentro del mismo equipo. Si intento acceder a esa dirección desde mi celular, este buscará un servidor en sí mismo, no en mi computador. Esto es un problema cuando los dispositivos no están en la misma red o cuando hay restricciones como firewalls.

Dev Tunnels resuelve este problema creando una URL pública accesible desde Internet, que redirige las peticiones hacia el servidor local corriendo en el puerto 3000. Conceptualmente, funciona como un intermediario seguro entre el mundo exterior y mi entorno local, permitiendo que dispositivos externos accedan a mi aplicación como si estuviera en línea.

### ¿Por qué usamos `JSON.stringify()` en el emisor (móvil) y `JSON.parse()` en el receptor (escritorio)? ¿Qué problema resuelve JSON aquí?

Usamos `JSON.stringify()` para convertir un objeto JavaScript (por ejemplo, que contiene las coordenadas del toque) en una cadena de texto en formato JSON, ya que los datos enviados por sockets deben ser strings o buffers. Esto permite transmitir estructuras de datos complejas de forma clara y estándar.

En el receptor, `JSON.parse()` toma esa cadena y la convierte nuevamente en un objeto JavaScript, para poder acceder fácilmente a sus propiedades. JSON resuelve el problema de enviar datos estructurados entre dispositivos en forma de texto legible y reutilizable.

### Función de `touchMoved()` y por qué se usa la variable `threshold` en el cliente móvil.

`touchMoved()` es una función de p5.js que se ejecuta automáticamente mientras el usuario mantiene el dedo en la pantalla y lo mueve. Es útil para detectar arrastres o gestos continuos.

La variable `threshold` se utiliza para limitar la cantidad de eventos enviados por la red. Si el movimiento detectado es menor que el umbral definido, no se envía ningún dato. Esto evita la saturación de la red por movimientos muy pequeños o temblores del dedo, y hace que el sistema sea más eficiente.

### ¿Qué otros eventos táctiles existen en p5.js y para qué tipo de interacciones podrían ser útiles?

* `touchStarted()`: Se activa cuando el usuario toca la pantalla por primera vez. Útil para detectar el inicio de una interacción, como presionar un botón.
* `touchEnded()`: Se ejecuta cuando el usuario levanta el dedo. Puede usarse para confirmar acciones o finalizar una animación.
* `touches[]`: Es un arreglo que contiene información sobre todos los toques activos. Es útil para implementar multitouch o gestos con más de un dedo.

### Ventajas y desventajas de cada uno

| Método      | Ventajas                                                 | Desventajas                                                                                   |
| ----------- | -------------------------------------------------------- | --------------------------------------------------------------------------------------------- |
| IP local    | No necesita Internet, funciona con baja latencia         | Solo funciona si los dispositivos están en la misma red, y puede verse afectado por firewalls |
| Dev Tunnels | Permite acceder desde cualquier red, fácil de configurar | Requiere conexión a Internet y puede tener algo de latencia                                   |

