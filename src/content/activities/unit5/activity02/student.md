**Caso de estudio: micro:bit – Comunicación Binaria**

###  Primer experimento: Visualización de datos binarios en Texto
- Al visualizar los datos binarios enviados por el micro:bit en formato Texto, aparecieron caracteres extraños.
- Esto ocurre porque los datos ya no son texto ASCII sino bytes binarios, y los valores binarios no siempre representan caracteres visibles.

###  Segundo experimento: Visualización de datos binarios en Hexadecimal
- Al cambiar la visualización a "Todo en Hex", los datos se mostraron como combinaciones de números y letras (ej: `FF 12 34 01 00`).
- Esto se relaciona con el uso de `struct.pack('>2h2B', xValue, yValue, int(aState), int(bState))`, que empaqueta 6 bytes en total: dos enteros cortos (2 bytes cada uno) y dos bytes simples (1 byte cada uno).

### Ventajas y desventajas del formato binario
**Ventajas:**
- Reducción del tamaño de los datos enviados.
- Transmisión más rápida y eficiente.

**Desventajas:**
- Los datos son difíciles de leer directamente.
- Se requiere conocer el formato para interpretarlos correctamente.

### Tercer experimento: Envío de datos sólo al detectar un "shake"
- Cada vez que se detecta un "shake", se envían exactamente 6 bytes.
- Esto concuerda con el formato `>2h2B`.
- Cada dato enviado corresponde a: 2 bytes para acelerómetro X, 2 bytes para acelerómetro Y, 1 byte para el botón A, 1 byte para el botón B.

###  Representación de números positivos y negativos
- En el formato `h` de `struct.pack`, los números positivos y negativos se representan en complemento a dos.
- Un número positivo se codifica de forma directa; un número negativo aparece como un valor hexadecimal "grande".

###  Cuarto experimento: Comparación entre datos en binario y ASCII
- Primero se reciben los datos binarios (6 bytes).
- Luego, se recibe la versión en ASCII (ej: `-276,346,0,0\n`).
- El formato ASCII es mucho más fácil de leer pero ocupa más espacio en bytes.

###  Comparativa general

**Formato Binario:**
- Ventajas: Mayor eficiencia y menor uso de ancho de banda.
- Desventajas: Difícil de leer sin decodificar.

**Formato ASCII:**
- Ventajas: Legible y fácil de interpretar manualmente.
- Desventajas: Mayor tamaño de los datos transmitidos y más lenta la comunicación.

