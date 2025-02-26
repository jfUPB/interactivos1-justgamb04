![image](https://github.com/user-attachments/assets/1664c335-bc35-4c10-8117-41be5e1cd86a)
![image](https://github.com/user-attachments/assets/e8caf12d-fc89-4a36-8c8d-3713d10a30b6)
![image](https://github.com/user-attachments/assets/58616e45-0bef-4fa9-9871-bc253feafc68)
![image](https://github.com/user-attachments/assets/bddd739b-d6cb-410a-b29e-2afd2401af70)

### Mapeo de los botones al movimiento del círculo

En este proyecto, usamos los **botones A y B del Micro:bit** para controlar el movimiento del círculo en la pantalla de **p5.js**.  

##### 1. Micro:bit detecta las pulsaciones de los botones 
   - Si se presiona el botón **A**, el Micro:bit envía la letra `"A"` por el puerto serie.  
   - Si se presiona el botón **B**, el Micro:bit envía la letra `"B"`.  

##### 2. El navegador recibe estos datos usando la Web Serial API  
   - `navigator.serial` se encarga de leer la información enviada por el Micro:bit.  
   - Se detecta si el mensaje recibido es `"A"` o `"B"`.  

##### 3. El código de p5.js interpreta los datos y mueve el círculo
   - Si recibe `"A"`, se **resta** un valor a la posición en **X**, moviendo el círculo **a la izquierda**.  
   - Si recibe `"B"`, se **suma** un valor a la posición en **X**, moviendo el círculo **a la derecha**.  

![image](https://github.com/user-attachments/assets/9aa65af0-6903-4cc3-aa50-60239cb714a0)
![image](https://github.com/user-attachments/assets/de010084-a721-46dd-a76f-7db864c28b49)
![image](https://github.com/user-attachments/assets/c9f732e2-3ceb-4237-a951-5f4d7cc4d67e)


##### Explicación sencilla del proceso
1. Micro:bit detecta el botón presionado.  
2. Envía "A" o "B" por el puerto serie.  
3. El navegador lee los datos y ajusta la posición del círculo en p5.js.  
4. El círculo se mueve a la izquierda o derecha dependiendo del botón presionado.  

