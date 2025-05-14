# Consolidación

## Utilidad de la técnica de máquina de estados 

La **máquina de estados** es una técnica muy útil porque permite estructurar el código de forma clara y organizada. En lugar de tener un montón de condicionales dispersos por todo el programa, todo se maneja por estados bien definidos.  

### **Escalabilidad**  
- **Facilidad para agregar nuevas funciones**: Si en el futuro quiero añadir un nuevo estado, como un "modo mantenimiento" para la bomba, solo tengo que definirlo y agregar las transiciones necesarias sin afectar demasiado el código ya existente.  
- **Modularidad**: Cada estado maneja su propia lógica, así que si hay que cambiar algo, se hace en un solo lugar sin afectar todo lo demás.  
- **Separación de eventos**: La máquina no necesita saber de dónde vienen los eventos (si del micro:bit o de p5.js), solo se encarga de responder a ellos.  

### **Concurrencia y manejo de eventos**  
- **Evita condiciones inesperadas**: Como solo hay transiciones definidas entre estados, no hay forma de que la bomba pase de "INACTIVA" a "EXPLODED" sin antes haber pasado por "ARMED".  
- **Mejor sincronización**: Al manejar eventos de forma ordenada, la aplicación no se enreda con entradas de usuario simultáneas. Por ejemplo, si alguien presiona **A** y al mismo tiempo llega un mensaje de p5.js, la máquina sabe exactamente cómo actuar sin conflictos.  
- **Responde rápido a los eventos**: No importa si el evento viene de un botón físico o de un mensaje serial, la bomba siempre reacciona de manera predecible.  

## Ventajas y desventajas

### **Ventajas**  
- **Aseguran que todo funcione correctamente**: Al probar cada estado con todos los eventos posibles, se reduce el riesgo de errores cuando la aplicación esté en uso.  
- **Facilitan encontrar errores antes de que sean un problema**: En los vectores de prueba, detecté fallos como el de activar la bomba desde "EXPLODED" sin resetear. Si no hubiera hecho pruebas, ese error habría pasado desapercibido.  
- **Permiten validar cambios sin miedo**: Si en el futuro quiero cambiar la interfaz en p5.js o mejorar la interacción con el micro:bit, puedo ejecutar las pruebas otra vez y verificar que todo siga funcionando bien.  

### **Desventajas**  
- **Consumen tiempo**: Probar todos los casos toma bastante trabajo, especialmente cuando hay que hacer pruebas de regresión.  
- **Dependen de la calidad de los casos de prueba**: Si no se consideran todos los escenarios posibles, algunos errores pueden pasar desapercibidos.  
- **Pueden ser tediosas**: A veces, repetir pruebas cada vez que se modifica algo puede ser aburrido, pero es necesario para garantizar que no se dañó nada en el proceso.  

## Importancia de las pruebas de regresión

Las **pruebas de regresión** son clave porque garantizan que los cambios en el código no rompan lo que ya funcionaba. A veces uno hace un cambio pequeño y sin darse cuenta afecta otras partes del programa.  

Si no hago **pruebas de regresión** después de modificar algo, pueden pasar cosas como:  
- **Errores ocultos**: Puede que al arreglar un bug, termine dañando otra parte del código sin darme cuenta.  
- **Comportamiento inesperado**: Por ejemplo, si cambio la forma en que se manejan los eventos y no pruebo todo de nuevo, puede que la bomba ya no explote cuando el tiempo llega a 0.  
- **Más tiempo perdido a futuro**: Si un error pasa desapercibido, luego tendré que pasar más tiempo tratando de encontrar qué fue lo que falló y en qué parte del código.  

Hacer pruebas de regresión es **como asegurarse de que todo sigue funcionando después de mover algo**. Es mejor hacerlas de una vez que lidiar con un bug más adelante cuando ya todo está integrado.  

## Conclusión 
El uso de una **máquina de estados** hizo que la implementación de la bomba fuera mucho más ordenada, escalable y fácil de mantener. Además, las **pruebas de vectores** ayudaron a detectar errores y asegurarse de que todo el sistema respondiera bien a cualquier evento, sin importar si venía del micro:bit o de p5.js.  

Las **pruebas de regresión** son una parte esencial en el desarrollo porque evitan que cambios pequeños dañen la funcionalidad existente. Aunque hacer pruebas puede ser tedioso, es la mejor forma de garantizar que la aplicación funcione bien en cualquier situación.  
