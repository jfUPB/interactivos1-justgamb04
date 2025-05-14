### Sitio 1: OpenProcessing

**Enlace del ejemplo:** citeturn0search3

**¿Qué me llamó la atención?:**
Este ejemplo me impresionó por su simplicidad y elegancia al crear líneas radiales que convergen en el centro, generando un efecto visual dinámico y atractivo.

**¿Cómo está hecho?:**
- **Funciones de p5.js utilizadas:**
  - `setup()`: Configura el lienzo y establece parámetros iniciales.
  - `draw()`: Dibuja continuamente las líneas radiales.
  - `translate()`: Mueve el origen de coordenadas al centro del lienzo.
  - `rotate()`: Rota el sistema de coordenadas para posicionar cada línea.
  - `line()`: Dibuja las líneas desde el centro hacia el borde del lienzo.

- **Técnicas de programación aplicadas:**
  - Uso de bucles para generar múltiples líneas con diferentes ángulos.
  - Transformaciones del sistema de coordenadas para simplificar el dibujo de elementos radiales.

**Modificación personal:**
- **Cambios realizados:**
  - Modifiqué el grosor de las líneas utilizando la función `strokeWeight()` para que aumente gradualmente desde el centro hacia afuera.
  - Agregué una variación de color en las líneas basada en su ángulo, utilizando la función `stroke()` junto con `map()` para asignar colores en el espectro HSB.

- **Motivación detrás de los cambios:**
  - Quería añadir una dimensión adicional al efecto visual, haciendo que las líneas no solo variaran en grosor sino también en color, creando una experiencia más rica y atractiva.

**Mi versión en p5.js:** [<iframe src="https://editor.p5js.org/just_gamb04/full/8Rg9SEiDH"></iframe>](https://editor.p5js.org/just_gamb04/sketches/8Rg9SEiDH)

### Sitio 2: Generative Design

**Enlace del ejemplo:** citeturn0search1

**¿Qué me llamó la atención?:**
Este ejemplo destaca por su capacidad para generar patrones geométricos complejos a partir de formas simples, demostrando el poder del diseño generativo.

**¿Cómo está hecho?:**
- **Funciones de p5.js utilizadas:**
  - `setup()`: Configura el lienzo y define parámetros iniciales.
  - `draw()`: Ejecuta el dibujo continuo de los patrones.
  - `rect()`: Dibuja rectángulos que conforman el patrón.
  - `push()` y `pop()`: Guardan y restauran el estado del sistema de coordenadas para manejar transformaciones anidadas.

- **Técnicas de programación aplicadas:**
  - Uso de recursión para crear subdivisiones dentro de una cuadrícula, generando patrones más complejos.
  - Aplicación de transformaciones geométricas para rotar y escalar elementos dentro del patrón.

**Modificación personal:**
- **Cambios realizados:**
  - Introduje una interacción con el mouse donde la profundidad de la recursión (y, por ende, la complejidad del patrón) aumenta o disminuye según la posición horizontal del cursor.
  - Implementé una paleta de colores alterna que cambia dinámicamente con la posición vertical del cursor.

- **Motivación detrás de los cambios:**
  - Busqué hacer el patrón más interactivo y permitir al usuario explorar diferentes niveles de complejidad y combinaciones de colores moviendo el cursor, fomentando una experiencia más inmersiva.

**Mi versión en p5.js:** [[https://editor.p5js.org/tu_usuario/sketches/tu_sketch_id](https://editor.p5js.org/tu_usuario/sketches/tu_sketch_id)](https://editor.p5js.org/just_gamb04/sketches/qjzsdJfVf)

### Sitio 3: p5.js Examples

**Enlace del ejemplo:** citeturn0search2

**¿Qué me llamó la atención?:**
Me atrajo la simplicidad del ejemplo y cómo demuestra claramente la interacción básica con el mouse para modificar elementos en el lienzo.

**¿Cómo está hecho?:**
- **Funciones de p5.js utilizadas:**
  - `setup()`: Establece el tamaño del lienzo y otros parámetros iniciales.
  - `draw()`: Dibuja continuamente una elipse cuya posición y tamaño dependen de la posición del mouse.
  - `ellipse()`: Dibuja la elipse en las coordenadas especificadas.
  - `map()`: Mapea la posición del mouse a un rango de valores para determinar el tamaño de la elipse.

- **Técnicas de programación aplicadas:**
  - Uso de la posición del mouse para controlar propiedades visuales de los elementos en el lienzo.
  - Aplicación de la función `map()` para escalar valores de entrada a rangos deseados.

**Modificación personal:**
- **Cambios realizados:**
  - Reemplacé la elipse por un rectángulo cuya rotación también depende de la posición del mouse, creando una sensación de perspectiva y profundidad.
  - Añadí una función que cambia el color de fondo cuando se presiona una tecla, permitiendo alternar entre diferentes esquemas de color.

- **Motivación detrás de los cambios:**
  - Quería explorar cómo la interacción del usuario puede afectar múltiples propiedades visuales simultáneamente y proporcionar una experiencia más dinámica y atractiva.

**Mi versión en p5.js:** [[https://editor.p5js.org/tu_usuario/sketches/tu_sketch_id](https://editor.p5js.org/tu_usuario/sketches/tu_sketch_id)](https://editor.p5js.org/just_gamb04/sketches/PDnMbqTB0)
