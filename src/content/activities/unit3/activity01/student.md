### **Código en MicroPython**  

```python
from microbit import *

class Semaforo:
    def __init__(self, tiempos, posicion):
        self.tiempos = tiempos
        self.posicion = posicion
        self.estado = "ROJO"
        self.tiempo_restante = self.tiempos["ROJO"]
        self.contador = 0  
    
    def actualizar(self):
        self.contador += 1

        if self.contador >= self.tiempo_restante:
            self.cambiar_estado()
            self.contador = 0  

        self.mostrar_estado()

    def cambiar_estado(self):
        if self.estado == "ROJO":
            self.estado = "AMARILLO"
            self.tiempo_restante = self.tiempos["AMARILLO"]
        elif self.estado == "AMARILLO":
            self.estado = "VERDE"
            self.tiempo_restante = self.tiempos["VERDE"]
        elif self.estado == "VERDE":
            self.estado = "ROJO"
            self.tiempo_restante = self.tiempos["ROJO"]

    def mostrar_estado(self):
        display.clear()
        if self.estado == "ROJO":
            display.set_pixel(self.posicion, 0, 9)  
        elif self.estado == "AMARILLO":
            display.set_pixel(self.posicion, 2, 5)  
        elif self.estado == "VERDE":
            display.set_pixel(self.posicion, 4, 2)  

tiempos_semaforo1 = {"ROJO": 5, "AMARILLO": 2, "VERDE": 3}
tiempos_semaforo2 = {"ROJO": 3, "AMARILLO": 1, "VERDE": 2}
tiempos_semaforo3 = {"ROJO": 4, "AMARILLO": 3, "VERDE": 2}

semaforo1 = Semaforo(tiempos_semaforo1, 0)
semaforo2 = Semaforo(tiempos_semaforo2, 2)
semaforo3 = Semaforo(tiempos_semaforo3, 4)

while True:
    semaforo1.actualizar()
    semaforo2.actualizar()
    semaforo3.actualizar()
    sleep(1000)  
```

### **Ventajas de Usar Clases para los Semáforos**  

La verdad usar clases en este caso ayuda bastante porque hace que el código sea más ordenado y fácil de manejar. En vez de estar escribiendo el código de cada semáforo por aparte, simplemente se crea una clase con todo lo necesario y después se usa varias veces sin tanto problema.  

Además, si en algún punto toca cambiar algo, como agregar otro estado o modificar los tiempos, no hay que ir línea por línea en diferentes partes del código, sino que solo se ajusta en la clase y listo. También hace que el código no sea tan largo y repetitivo, lo cual es clave porque así es más fácil de entender y depurar cuando algo no funciona bien.  

Otro punto es que al usar clases se siente más como programar cosas de la vida real. O sea, cada semáforo es como un objeto independiente que tiene su propio tiempo y estado sin afectar a los otros. Esto hace que sea más organizado y que el programa no se enrede cuando hay varios semáforos funcionando al mismo tiempo.  

### **Reflexión sobre la Técnica de Máquinas de Estado**  

La idea de usar máquinas de estado es muy útil porque ayuda a que todo siga un orden lógico. Cada semáforo tiene estados específicos (rojo, amarillo, verde) y cambia entre ellos en un orden claro, lo que hace que el código sea más estructurado.  

Me di cuenta que programar de esta forma también sirve para un montón de cosas aparte de semáforos, por ejemplo en videojuegos cuando hay que hacer que un personaje pase de caminar a correr o a saltar, o en interfaces donde los botones cambian de estado según lo que el usuario haga.  

Al final, siento que este tipo de programación ayuda a que el código sea más claro y no se vuelva un desastre cuando hay varias cosas pasando al mismo tiempo. Al principio fue medio enredado entender bien cómo hacer que los tres semáforos funcionaran al mismo tiempo sin pisarse entre ellos, pero ya cuando le agarré la lógica con los estados y los tiempos.
