https://editor.p5js.org/just_gamb04/sketches/A1GqH-Gv1

### ¿Cómo funciona el sketch?

Este sketch genera un conjunto de líneas verticales que cambian de color, grosor y posición dependiendo de la cantidad de líneas que se generan. Visualmente, se crea un patrón que parece casi como una textura digital o una especie de código de barras artístico.

El código está organizado así:

- **En `setup()`** se define el tamaño del lienzo (`createCanvas(800, 800)`) y se configura el modo de color HSB para tener más control sobre el matiz y la saturación de los colores.
- **En `draw()`** se limpia el fondo y se dibujan las líneas en un bucle. Cada línea tiene un grosor y color diferentes. Se usan funciones como `strokeWeight()` y `stroke()` para cambiar el estilo de cada línea.
- Hay también una parte del código que permite que al presionar teclas se puedan cambiar los parámetros para redibujar los patrones.

### ¿Qué técnicas usa el código?

- Usa bucles `for` para repetir las líneas.
- Usa funciones de p5.js como `strokeWeight()`, `stroke()`, `line()` y `colorMode()`.
- Se basa mucho en la idea de variar parámetros como color, posición y grosor usando `map()` y funciones matemáticas.
- También tiene controles para que con ciertas teclas se reinicie el dibujo con diferentes variaciones.

### Mis dibujos y modificaciones

Estuve probando varias combinaciones cambiando los valores directamente en el código. Algunas de las cosas que hice:

1. **Aumenté el número de líneas**  
   Cambié el valor de la variable `count` para que en lugar de 20 líneas se hicieran 100. Esto hizo que el patrón se viera más denso y detallado.

2. **Modifiqué los colores**  
   Ajusté los valores del matiz (hue) en `stroke()` para que los colores tuvieran una gama más cálida (usando naranjas y rojos en lugar de colores al azar).

3. **Agregué interacción con el mouse**  
   Añadí una función `mousePressed()` que cambia el grosor de las líneas cada vez que se hace clic. Eso le dio más dinamismo al sketch.
