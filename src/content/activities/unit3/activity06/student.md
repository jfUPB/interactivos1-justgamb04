## **Vectores de prueba**  

### **Pruebas iniciales**  

| **#** | **Estado actual** | **Evento recibido** | **Qué debería pasar** | **Estado esperado** | **Pasó o no pasó** |
|------|----------------|----------------|----------------|----------------|----------------|
| 1 | CONFIG: Ajuste de tiempo | Shake (S) | Se arma la bomba y empieza a contar | ARMED: Contando tiempo | [X] |
| 2 | CONFIG: Ajuste de tiempo | Botón B | Se disminuye el tiempo | CONFIG: Ajuste de tiempo | [X] |
| 3 | CONFIG: Ajuste de tiempo | RESET desde p5.js | Se resetea el tiempo y queda en config | CONFIG: Ajuste de tiempo | [X] |
| 4 | ARMED: Contando tiempo | Tiempo llega a 0 | La bomba explota | EXPLODED: Bomba explotada | [X] |
| 5 | ARMED: Contando tiempo | Botón Touch T | Se resetea y vuelve a configuración | CONFIG: Ajuste de tiempo | [X] |
| 6 | EXPLODED: Bomba explotada | Cualquier evento | No debería hacer nada, ya explotó | EXPLODED: Bomba explotada | [X] |
| 7 | ARMED: Contando tiempo | RESET desde p5.js | Debería volver a configuración | CONFIG: Ajuste de tiempo | [ ] |


### **Errores que me salieron y cómo los arreglé**  

#### **Error en el vector #7**  
**¿Qué pasó?**  
Cuando la bomba ya estaba en **ARMED** y le mandaba el reset desde p5.js, no volvía a **CONFIG**. Se quedaba ahí como si nada.  

**¿Cómo lo arreglé?**  
- Me fui a revisar la máquina de estados y vi que no estaba controlando el reset en el estado **ARMED**.  
- Agregué una condición para que cuando llegue el evento de **RESET**, pase directo a **CONFIG**.  
- También revisé que en p5.js estuviera mandando bien el evento por el serial, por si acaso.  


### **Volviendo a probar todo (Pruebas de regresión)**  
Después de meter el arreglo, volví a probar **todos los vectores** desde el inicio y al final ya todo pasó bien, y el sistema quedó funcionando como debe.  
