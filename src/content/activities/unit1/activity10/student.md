![image](https://github.com/user-attachments/assets/8413b15d-4560-4179-95b9-c4fc9251361c)

![image](https://github.com/user-attachments/assets/b16e5f8c-f668-4924-a0e6-6fb1ff00da76)

![image](https://github.com/user-attachments/assets/4aa8c123-cbf2-4150-9c36-f375e2596117)

![image](https://github.com/user-attachments/assets/fbb45b04-54fb-494c-8242-9c8e1892f3e3)

### **Proceso de conexión y comunicación entre Micro:bit y p5.js**  

Para esta actividad, se utilizó **p5.js** para generar un gráfico interactivo y un **Micro:bit** para enviar señales que modifican el comportamiento visual en la pantalla. La comunicación se realiza a través de la **Web Serial API**, lo que permite que el navegador reciba datos desde el Micro:bit cuando un botón es presionado.  

---

### **1. Conectando el Micro:bit con p5.js**  
1. **Programación del Micro:bit:**  
   - Se cargó un código en **MicroPython** usando [https://python.microbit.org/v/3](https://python.microbit.org/v/3).  
   - El código hace que el Micro:bit envíe un mensaje `"cambio"` por el puerto serie cuando se presiona el botón **A**.  

2. **Configuración en p5.js:**  
   - Se creó un archivo **index.html** con un botón `Conectar Micro:bit`.  
   - Un archivo **sketch.js** maneja la comunicación y actualiza el color de un cuadrado en la pantalla cuando se recibe el mensaje `"cambio"`.  

3. **Establecimiento de la conexión:**  
   - Al hacer clic en `Conectar Micro:bit`, el navegador solicita acceso al puerto USB del Micro:bit.  
   - Una vez conectado, p5.js empieza a recibir datos enviados por el Micro:bit.  

---

### **2. Comunicación entre Micro:bit y p5.js**  
- **Micro:bit:**  
  - Envía `"cambio"` cuando se presiona el botón **A**.  
- **p5.js:**  
  - Detecta el mensaje y ejecuta `serialEvent()`, que cambia el color del cuadrado en pantalla.  

---

### **3. Flujo de ejecución**  
1. **Se presiona el botón "Conectar Micro:bit".**  
2. **El usuario selecciona el Micro:bit en la lista de dispositivos disponibles.**  
3. **El Micro:bit envía `"cambio"` al presionar el botón A.**  
4. **p5.js recibe el mensaje y cambia el color del cuadrado.**  
5. **El proceso se repite mientras la conexión esté activa.**  

---

### **4. Conclusión**  
Esta actividad permitió entender cómo usar la **Web Serial API** para conectar hardware con gráficos interactivos en p5.js. Se experimentó con la recepción de datos en tiempo real y su aplicación en la modificación de elementos visuales, combinando software y hardware de manera práctica. 🚀
