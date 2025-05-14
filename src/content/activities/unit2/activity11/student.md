![Untitled diagram-2025-03-19-143745](https://github.com/user-attachments/assets/39005be9-85b8-487f-ae0e-f66d9f080cbb)

### **Explicación clara del diagrama**  
- **CONFIGURACIÓN**:  
  - Estado inicial.  
  - Se ajusta el tiempo con **UP (+1s)** y **DOWN (-1s)** dentro del rango de 10 a 60 segundos.  
  - **Shake (ARMED)** cambia al estado **ARMADA**.  

- **ARMADA**:  
  - Se inicia la cuenta regresiva.  
  - Se muestra en la pantalla de LEDs.  
  - Si el tiempo llega a **0**, pasa a **EXPLOSIÓN**.  
  - Si se presiona **TOUCH**, vuelve a **CONFIGURACIÓN**.  

- **EXPLOSIÓN**:  
  - Activa el **speaker**.  
  - Solo se puede reiniciar con **TOUCH**, regresando a **CONFIGURACIÓN**.  
