
#### **1. URL de Dev Tunnels obtenida**

* URL obtenida: ****
* Necesitamos usar esta URL en lugar de `http://localhost:3000` o la IP local de mi computador porque Dev Tunnels crea un túnel de red público, lo que permite que dispositivos externos (en este caso, el celular) accedan a una aplicación que se ejecuta localmente en mi computador. `localhost` y la IP local solo son accesibles desde mi computadora, mientras que Dev Tunnels expone el servidor de forma segura y accesible desde cualquier lugar, incluyendo el celular.

#### **2. Descripción de los comandos `npm install` y `npm start`**

* **`npm install`**: Este comando se utiliza para instalar todas las dependencias definidas en el archivo `package.json` de un proyecto Node.js. En este caso, instala las dependencias necesarias como Express y Socket.IO para que el proyecto funcione correctamente.
* **`npm start`**: Este comando se utiliza para iniciar el servidor de Node.js. Ejecuta el archivo definido en el script de inicio del proyecto (generalmente `server.js`) y pone el servidor en funcionamiento, lo que permite que los clientes (navegador de computadora y celular) se conecten al servidor a través del puerto especificado.

#### **3. Mensajes observados en la terminal del servidor**

* Los mensajes que observé en la terminal del servidor fueron los siguientes:

  * **“New client connected”**: Este mensaje aparece cuando un nuevo cliente (ya sea el computador o el celular) se conecta al servidor.
  * **“Received message => …”**: Este mensaje aparece cuando el servidor recibe datos del cliente. Muestra la información enviada desde el cliente.
  * **“Client disconnected”**: Este mensaje aparece cuando un cliente se desconecta, como cuando cierro una de las pestañas en el computador o el celular.
* Los identificadores y los mensajes eran similares para ambos clientes (escritorio y móvil), pero estaba claro que el servidor identificaba de forma separada las conexiones de los diferentes clientes.

#### **4. Comportamiento observado**

* **¿Funcionó la interacción?**: Sí, la interacción funcionó correctamente. El círculo en el navegador de mi computador seguía los movimientos de mi dedo en la pantalla del celular.
* **¿Hubo retraso (latencia)?**: No noté ningún retraso significativo. La comunicación entre el celular y el computador parecía casi en tiempo real, lo cual fue bastante fluido.

#### **5. Cierre del puerto**

* **¿Cerré el puerto?**: Sí, cerré el puerto después de realizar las pruebas.
* **¿Por qué es importante cerrar el puerto?**: Es importante cerrar el puerto para evitar accesos no autorizados y liberar recursos de red que ya no son necesarios. Mantener un puerto abierto innecesariamente podría ser un riesgo de seguridad, ya que permitiría que otros dispositivos o usuarios accedieran a la aplicación sin control.


### Observaciones:
* **Comportamiento**: Captura de pantalla del círculo moviéndose en el navegador de mi computador y el mensaje “Touch to move the circle” en el móvil.
