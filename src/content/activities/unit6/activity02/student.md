## ¿Qué pasaría si se corta mi rampa de acceso a Internet?

Me conecto por Wi-Fi tanto en casa como en la universidad. Si se corta esa conexión, no podría acceder a páginas web, servicios en línea, plataformas educativas ni enviar trabajos. Sería como si mi vehículo (el navegador) quedara atrapado sin poder entrar a la carretera (Internet).

## Ejemplos de relaciones Cliente-Servidor en la vida diaria

- Restaurante: el cliente es la persona que hace el pedido, y el servidor es el mesero que lo lleva a la cocina y luego trae la comida.
- Biblioteca: el usuario pide un libro (cliente) y el bibliotecario (servidor) lo busca y lo entrega.
- Cajero automático: el cliente pide un retiro o consulta, el sistema del banco procesa la solicitud y entrega la información o el dinero.

## Desglose de una URL

URL: `https://www.youtube.com/watch?v=abc123`

- **Protocolo:** `https://`
- **Nombre de dominio:** `www.youtube.com`
- **Ruta:** `/watch?v=abc123`

Si solo escribo `www.youtube.com`, el servidor me envía una página por defecto, normalmente la página principal con videos recomendados, como si fuera el “inicio” del sitio.

## Comparación entre HTTP y los protocolos seriales

**Similitudes:**
- Ambos definen reglas de comunicación.
- Tienen un emisor y un receptor.
- Se necesita una estructura clara para que el mensaje se entienda.

**Diferencias:**
- HTTP es más complejo, con encabezados, códigos de estado, tipos de contenido, etc.
- En protocolos seriales simples como el del micro:bit, se envían datos en bruto (bytes) sin tanto contexto.

**¿Por qué HTTP es más complejo?**
Porque necesita transmitir mucho más que simples valores: archivos, páginas web completas, identificarse como navegador, decir qué formato espera, etc.

## Componentes de una página web de login

- **HTML:** los campos de usuario y contraseña, el botón de “Iniciar sesión”.
- **CSS:** el color del botón, la fuente del texto, el fondo de la página.
- **JavaScript:** validar que los campos no estén vacíos, mostrar un mensaje si la contraseña es incorrecta sin recargar la página.

## ¿Por qué usar JavaScript tanto en el cliente como en el servidor?

- Permite usar un solo lenguaje para ambos lados, lo cual hace el desarrollo más fácil y rápido.
- Los desarrolladores no tienen que aprender dos lenguajes distintos.
- Facilita la comunicación entre cliente y servidor, porque ambos “piensan” en JavaScript.

## Comparación entre draw() y el modelo basado en eventos

**Ventajas del modelo basado en eventos:**
- Es más eficiente: solo se ejecuta código cuando realmente ocurre algo (clic, mensaje, cambio de ventana).
- Ahorra recursos porque no redibuja todo constantemente.
- Es más adecuado para interfaces web que responden a interacciones del usuario.

**¿Sería eficiente tener un bucle draw()?**
No. Redibujar toda la página 60 veces por segundo sin necesidad consumiría muchos recursos del navegador y del sistema.

## Diferencia entre HTTP y WebSockets/Socket.IO

- **HTTP:** es como enviar una carta, esperar respuesta y terminar. Es una comunicación de una sola vez por cada petición.
- **WebSockets/Socket.IO:** es como una llamada telefónica: se establece una conexión y ambos pueden enviarse mensajes en tiempo real, sin volver a hacer una “petición” cada vez.

**Aplicaciones en tiempo real:**
- Chats en línea.
- Juegos multijugador en navegador.
- Aplicaciones de colaboración como Google Docs.
- Monitoreo en vivo (bolsa, sensores, etc.).
