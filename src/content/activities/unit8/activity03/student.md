### Brainstorming – 3 Ideas Iniciales

**1. Jardín interactivo de emociones**

* *Narrativa:* Un espacio digital donde las emociones se visualizan como flores y su crecimiento depende del movimiento del micro\:bit (suavidad = florecer, brusquedad = marchitar) y de la intensidad del color que envía el móvil (como una “aura emocional”).
* *Inputs:* Micro\:bit (Serial) mide aceleración; Móvil (Socket) envía color elegido por el usuario.
* *Output:* Flores que crecen y cambian su forma/color dependiendo de las emociones combinadas.

**2. Lienzo caótico**

* *Narrativa:* Un canvas donde el micro\:bit controla la dirección del trazo y el móvil añade “eventos aleatorios” (como explosiones de color).
* *Inputs:* Micro\:bit (movimiento), Móvil (toques en pantalla que generan perturbaciones visuales).
* *Output:* Una obra en constante mutación entre orden y caos, dependiendo del equilibrio entre los inputs.

**3. Ecosistema digital**

* *Narrativa:* Un ecosistema visual donde el micro\:bit representa el clima (movimiento = tormenta o calma) y el móvil actúa como “humano” que interviene (daños o cuidado).
* *Inputs:* Micro\:bit (movimiento como intensidad del viento), Móvil (botones para dar agua o causar incendio).
* *Output:* Animales o plantas que reaccionan al “clima” y a la intervención humana.

### Concepto I-P-O Final

**Título provisional:** *Lienzo Caótico: La danza del orden y el caos*

**Narrativa/Concepto central:**
Una obra digital viva que se construye con el equilibrio entre el movimiento (micro\:bit) y la interacción emocional del usuario (móvil). El micro\:bit, sostenido en la mano o el cuerpo, actúa como pincel maestro que guía la dirección y forma de trazos en un canvas de p5.js, mientras que el móvil, usado por otra persona o por el mismo usuario, interfiere con impulsos repentinos que generan caos visual. El proyecto explora el balance entre control y aleatoriedad, entre intención y accidente.

### Inputs específicos

* **Micro\:bit (Serial):**

  * *Sensor:* acelerómetro.
  * *Uso:* Dirección del trazo en pantalla (izquierda/derecha/arriba/abajo) y su velocidad.
  * *Justificación:* Proporciona un control físico y gestual, expresivo y en tiempo real.

* **Móvil (Socket vía Node.js):**

  * *Sensor/Interacción:* Eventos de toque o deslizamiento en pantalla.
  * *Uso:* Provocar estallidos de color, alterar el grosor del trazo, introducir ruido visual.
  * *Justificación:* Añade un componente emocional, repentino, que interrumpe la calma del movimiento.

* **Computador (local - p5.js):**

  * *Interacción:* Teclado para cambiar modos (blanco y negro, modo pintura abstracta, etc.)
  * *Justificación:* Permite alternar entre estilos o paletas, agregando más control creativo.

### Process (Lógica en p5.js)

El sketch de p5.js recibirá constantemente los datos del micro\:bit (Serial) y del móvil (Socket.IO). El sistema mantendrá un *estado actual del trazo* que incluye: dirección, velocidad, color y nivel de caos. El micro\:bit controlará directamente la posición del pincel virtual, aplicando aceleración para modificar la velocidad y dirección del movimiento. El móvil enviará eventos de "golpe visual" que alterarán el entorno: explosiones de color, ruidos visuales o distorsiones en el trazo. Un sistema de variables internas (como `trazoX`, `trazoY`, `caosNivel`, `modoEstilo`) manejará estas transformaciones.

**Variables clave:**

* `trazoX`, `trazoY`: posición actual del pincel.
* `velocidadX`, `velocidadY`: derivadas del micro\:bit.
* `caosNivel`: aumenta con cada toque desde el móvil.
* `modoEstilo`: cambia con teclas del teclado.

Se aplicarán reglas como:

* Si el caos es alto, el trazo se vuelve más grueso e inestable.
* Si el micro\:bit se mueve con suavidad, los trazos son lineales y fluidos.
* Cada toque del móvil genera una ráfaga radial de color desde la posición del trazo.

### Output deseado

**Visualización/interacción resultante:**
En la pantalla se ve un lienzo digital donde el usuario puede "pintar con el aire" usando el micro\:bit. El trazo sigue el movimiento del micro\:bit, y cada toque en el móvil genera estallidos de color, como si el lienzo estuviera siendo intervenido desde otra dimensión. Con el teclado se puede alternar entre estilos: abstracto, monocromático, glitch, etc.

**Boceto**
Imagina una línea que se mueve suavemente por la pantalla, cambiando de color según el estado emocional del móvil. A veces se distorsiona, se rompe o se engrosa, dependiendo de la intensidad del caos. El lienzo guarda todas estas marcas, generando una pintura viva que evoluciona con cada interacción.
