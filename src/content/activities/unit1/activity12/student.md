![image](https://github.com/user-attachments/assets/3859aae3-88e0-42c5-a0c9-7ae3bdef18e5)
![image](https://github.com/user-attachments/assets/6fd69c2a-4af6-463a-8c13-67fc2aea4e2b)
![image](https://github.com/user-attachments/assets/70e3bc1d-15d1-49e6-be75-2c080dfbc062)
![image](https://github.com/user-attachments/assets/2e2de9af-bc0f-467c-9a64-9340690ec438)


Para solucionar el problema de carga de imágenes en p5.js, primero revisé que los archivos de imagen estuvieran en la misma carpeta que el código. Noté que p5.js no encontraba las imágenes porque estaban en una ubicación incorrecta.  

Lo solucioné de esta manera:  
##### 1. Moví las imágenes a la misma carpeta donde está el archivo `sketch.js`.  
##### 2. Verifiqué los nombres de los archivos, asegurándome de que coincidieran exactamente con lo que puse en el código (incluyendo mayúsculas y minúsculas).  
##### 3. Si las imágenes estaban en otra carpeta, cambié el código para indicar la ruta correcta, por ejemplo:  
   ```javascript
   images[1] = loadImage("images/img1.png");
   ```

Al final, la solución fue asegurarme de que el código y las imágenes estuvieran bien organizados y accesibles.
