## Objetivo de la actividad

El objetivo fue desarrollar una aplicación interactiva en p5.js que utilice datos recibidos desde un micro:bit mediante comunicación serial. Se debía implementar una estructura de estados, diseñar una meta o reto para el usuario, y analizar el código desarrollado a lo largo del proceso.

## Proceso de desarrollo

### Recepción de datos desde el micro:bit

Para iniciar el desarrollo, conecté el micro:bit al computador mediante USB y programé el envío continuo de datos de aceleración en los ejes X e Y a través de la conexión serial. Estos datos fueron enviados como cadenas de texto con formato "x,y", por ejemplo: `300,-200`.

En p5.js utilicé la librería `p5.serialport` para recibir los datos en tiempo real. La función `serialEvent()` capturó los valores y los asignó a las variables `x` y `y`.

### Interpretación de los datos y control del personaje

Los valores recibidos fueron mapeados a las coordenadas de la pantalla. Con esto, un personaje (representado por un cuadrado rojo) se movía en función de la inclinación del micro:bit. El jugador podía controlar al personaje simplemente inclinando el dispositivo.

Esta interacción fue sencilla pero efectiva para generar un control físico dentro del entorno digital.

## Implementación de estados

Inicialmente, el programa no tenía una estructura de estados. Todo el juego ocurría directamente en el `draw()`, sin pantallas intermedias ni separación lógica entre fases. Para cumplir con este criterio, realicé una modificación al código incluyendo una variable `estado` que controla tres fases: `"inicio"`, `"jugando"` y `"fin"`.

La lógica de los estados quedó estructurada de la siguiente manera:

```javascript
let estado = "inicio";

function draw() {
  background(220);
  
  if (estado === "inicio") {
    mostrarPantallaInicio();
  } else if (estado === "jugando") {
    ejecutarJuego();
  } else if (estado === "fin") {
    mostrarPantallaFinal();
  }
}
```

Cada función (`mostrarPantallaInicio`, `ejecutarJuego`, `mostrarPantallaFinal`) controla los elementos visuales y lógicos de su respectiva fase. El juego inicia cuando se presiona una tecla, y pasa al estado "fin" cuando el tiempo se agota.

## Diseño del reto o meta del jugador

El objetivo del juego es recolectar la mayor cantidad de círculos (objetos) antes de que se acabe el tiempo. El jugador debe mover el personaje con el micro:bit e intentar "colisionar" con los círculos, los cuales desaparecen y aumentan el puntaje al ser recogidos.

El juego tiene un contador de tiempo que inicia en 30 segundos. Cuando el tiempo se acaba, el juego muestra una pantalla final con el puntaje obtenido.

## Interfaz visual

La interfaz visual es sencilla pero funcional. Incluye los siguientes elementos:
- Pantalla de inicio con instrucciones.
- Durante el juego, se muestran el puntaje y el tiempo restante.
- En la pantalla final, se muestra un mensaje de "Juego terminado" junto al puntaje.

Se usaron figuras geométricas básicas y colores planos. Aunque la interfaz cumple con lo solicitado, podría mejorarse con iconografía, animaciones o estilos visuales más atractivos.

## Preguntas del enunciado

**1. ¿Qué datos estás enviando desde el micro:bit a p5.js?**  
Desde el micro:bit estoy enviando los valores de aceleración en los ejes X e Y. Estos valores representan la inclinación del dispositivo y se envían como una cadena de texto en formato "x,y" por la comunicación serial.

**2. ¿Qué uso le diste a esos datos en la aplicación?**  
Los datos se usaron para controlar el movimiento del personaje principal (un cuadrado rojo). A medida que se inclina el micro:bit, los valores cambian, y el personaje se desplaza en la pantalla de p5.js.

**3. ¿Cómo implementaste los estados en tu aplicación?**  
Implementé una variable llamada `estado` que define en qué parte del juego se encuentra la aplicación: "inicio", "jugando" o "fin". Según el valor de esta variable, se muestran diferentes pantallas y se ejecuta la lógica correspondiente. Esta estructura permite separar el flujo del juego y mejorar la organización del código.

## Hallazgos y aprendizajes

- Usar la aceleración del micro:bit como entrada es una manera intuitiva de introducir interactividad física a una aplicación digital.
- La estructura de estados facilita la organización del juego y permite escalar la aplicación más fácilmente.
- Es importante controlar errores en la comunicación serial, ya que los datos pueden llegar incompletos o corruptos si no se formatean correctamente.
- La sincronización entre p5.js y el micro:bit es fluida, pero requiere configurar correctamente el puerto serial y limpiar los datos recibidos con cuidado.

### **Código final (p5.js)**

```javascript
let serial;
let portName = '/dev/tty.usbmodemXXXX'; // Cambia esto por el nombre real del puerto
let x = 0;
let y = 0;
let jugador;
let objetos = [];
let puntaje = 0;
let tiempoTotal = 30; // duración en segundos
let tiempoInicio;
let estado = "inicio";

function setup() {
  createCanvas(600, 400);
  serial = new p5.SerialPort();
  serial.open(portName);
  serial.on('data', recibirDatos);

  jugador = createVector(width / 2, height / 2);
  generarObjetos(5);
}

function draw() {
  background(220);

  if (estado === "inicio") {
    mostrarPantallaInicio();
  } else if (estado === "jugando") {
    ejecutarJuego();
  } else if (estado === "fin") {
    mostrarPantallaFinal();
  }
}

function mostrarPantallaInicio() {
  textAlign(CENTER, CENTER);
  textSize(24);
  fill(0);
  text("Inclina el micro:bit para mover el cuadrado.\nRecoge los círculos antes de que se acabe el tiempo.\nPresiona cualquier tecla para comenzar.", width / 2, height / 2);
}

function ejecutarJuego() {
  let tiempoRestante = tiempoTotal - int((millis() - tiempoInicio) / 1000);
  if (tiempoRestante <= 0) {
    estado = "fin";
  }

  moverJugador();

  fill(255, 0, 0);
  rect(jugador.x, jugador.y, 40, 40);

  // Mostrar y verificar colisiones con los objetos
  for (let i = objetos.length - 1; i >= 0; i--) {
    fill(0, 100, 255);
    ellipse(objetos[i].x, objetos[i].y, 20, 20);

    if (dist(jugador.x, jugador.y, objetos[i].x, objetos[i].y) < 30) {
      objetos.splice(i, 1);
      puntaje++;
    }
  }

  fill(0);
  textSize(16);
  text("Puntaje: " + puntaje, 10, 20);
  text("Tiempo restante: " + tiempoRestante + "s", 10, 40);

  if (objetos.length === 0) {
    generarObjetos(5); // generar más objetos al terminar
  }
}

function mostrarPantallaFinal() {
  textAlign(CENTER, CENTER);
  textSize(24);
  fill(0);
  text("Juego terminado\nPuntaje final: " + puntaje + "\nPresiona cualquier tecla para reiniciar", width / 2, height / 2);
}

function keyPressed() {
  if (estado === "inicio") {
    estado = "jugando";
    puntaje = 0;
    generarObjetos(5);
    tiempoInicio = millis();
  } else if (estado === "fin") {
    estado = "inicio";
    jugador = createVector(width / 2, height / 2);
  }
}

function moverJugador() {
  // Mapear aceleración a velocidad de movimiento
  let velX = map(x, -1024, 1024, -5, 5);
  let velY = map(y, -1024, 1024, -5, 5);

  jugador.x += velX;
  jugador.y += velY;

  // Limitar dentro de la pantalla
  jugador.x = constrain(jugador.x, 0, width - 40);
  jugador.y = constrain(jugador.y, 0, height - 40);
}

function generarObjetos(cantidad) {
  objetos = [];
  for (let i = 0; i < cantidad; i++) {
    let ox = random(20, width - 20);
    let oy = random(20, height - 20);
    objetos.push(createVector(ox, oy));
  }
}

function recibirDatos() {
  let data = serial.readLine().trim();
  if (data.length > 0) {
    let valores = data.split(",");
    if (valores.length === 2) {
      x = int(valores[0]);
      y = int(valores[1]);
    }
  }
}
```

