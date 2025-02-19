![image](https://github.com/user-attachments/assets/6f5fa213-aa4c-4d33-a58e-d0a4d7555a92)

![image](https://github.com/user-attachments/assets/82b9f0f5-0ba6-4fe7-bcd7-4d07d8214098)

#### Descripción de las funciones utilizadas y modificaciones de parámetros  

En este código de p5.js, utilicé varias funciones matemáticas y de dibujo para generar patrones visuales de manera aleatoria.  

1. `random(min, max)`: Se usa para generar valores aleatorios dentro de un rango específico. En este caso, lo utilicé para variar el color azul de los círculos y hacer que cada ejecución del programa tenga un resultado diferente.  

2. `sin()` y `cos()`: Estas funciones trigonométricas me ayudan a crear desplazamientos en los círculos, haciendo que su posición cambie de manera oscilante. Esto le da un efecto más dinámico y menos rígido a la distribución de los patrones.  

3. `map(value, start1, stop1, start2, stop2)`: La utilicé para convertir los valores de `sin()` y `cos()` en un rango que pueda aplicarse a los colores, permitiendo una transición suave de tonalidades en la cuadrícula.  

4. `noLoop()`: Hace que el patrón se genere solo una vez en lugar de actualizarse constantemente, lo que es útil para obtener un diseño estático aleatorio en cada ejecución.  

5. `ellipse(x, y, size)`: Dibuja los círculos en la cuadrícula con tamaños proporcionales al espacio disponible.  

### Modificaciones de parámetros
- Cambié el número de filas y columnas para controlar la densidad del patrón.  
- Ajusté la velocidad de oscilación modificando el factor de `frameCount` dentro de `sin()` y `cos()`.  
- Alteré los colores con `map()` para que el patrón tenga una apariencia más vibrante y variada.  

