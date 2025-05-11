### Comprensión actual del modelo I-P-O

En este punto del curso, entiendo mucho mejor cómo se aplica el modelo Input-Process-Output (I-P-O) dentro de un sistema interactivo distribuido. Antes lo veía como una forma básica de estructurar el código, pero ahora, con la experiencia que tengo de integrar móvil, micro\:bit y computador, entiendo que es más una guía para organizar toda la arquitectura del sistema.

### Rol de cada componente en el esquema I-P-O

**Móvil:**
Funciona como una fuente de *input* remota, que se conecta a través de Socket.IO y Node.js para enviar datos como coordenadas o gestos.

**Micro\:bit:**
También actúa como fuente de *input*, pero conectada localmente al computador mediante comunicación serial. Puede enviar datos de sensores, botones, etc.

**Servidor Node.js (Puente):**
Aunque no realiza procesamiento, su rol es esencial. Sirve de puente entre el móvil y el computador. No transforma ni interpreta datos, solo los retransmite. Es como un mensajero que permite la conexión entre dispositivos que normalmente no podrían comunicarse directamente.

**Cliente p5.js (Computador):**
Aquí es donde sucede todo el *proceso*. Se combinan los datos recibidos del móvil (vía Node), del micro\:bit (vía serial) y del computador mismo (teclado, mouse). En este cliente se aplica la lógica de la experiencia interactiva: cómo se interpretan los datos, qué decisiones se toman, y qué respuesta visual se genera.

### Importancia de Node.js como puente

Aunque el servidor Node.js no procese datos directamente, es absolutamente necesario dentro de este esquema. Sin él, no podríamos recibir datos del móvil en el computador, porque no se puede conectar directamente. Node.js crea ese canal de comunicación que hace posible el flujo completo de información.

### Valor del modelo I-P-O para diseñar experiencias

El modelo I-P-O ayuda a tener claridad de responsabilidades dentro del sistema: cada parte cumple una función específica. Esto no solo hace más claro el diseño inicial, sino que también facilita el desarrollo y la depuración. Saber exactamente qué entra, cómo se procesa y qué debe salir permite diseñar experiencias coherentes y funcionales.


### Ideas iniciales de narrativa

Pensando en una narrativa para el proyecto final, me gustaría hacer una experiencia en la que el usuario del móvil controle ciertos “poderes” o herramientas, mientras que el micro\:bit modifique el entorno (por ejemplo, clima, tiempo, terreno). El teclado y el mouse servirían para moverse o explorar el escenario. Sería como una historia interactiva donde diferentes dispositivos se usan de manera cooperativa para crear una sensación de inmersión y participación activa.
