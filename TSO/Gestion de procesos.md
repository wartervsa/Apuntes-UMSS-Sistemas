En las computadoras actuales se manejan varios procesos a la vez, aunque estrictamente un procesador solo maneja un proceso a la vez, pero lo hace tan rápido, donde pudo cambiar entre programas en 1 segundo, que da la sensación de paralelismo(pseudoparalelismo). Ahora ésta rápida conmutación de un proceso a otro en algún orden se llama multiprogramación. 
**Un programa** es una entidad estática constituida por sentencias de programa que definen el comportamiento de los procesos cuando se ejecutan utilizando un conjunto de datos. 
**Un proceso** es la ejecución del programa, es una instancia con un conjunto de datos. 
Como por ejemplo sería como en programación orientada a objetos donde la clase sería el programa y los objetos serían los procesos. 
**Creación de un proceso**
Los sistemas operativos deben asegurar de alguna manera la existencia de todos los procesos que sean requeridos. El so está encargado de la ejecución y terminación de procesos mientras esté operando. De los sucesos principales para la creación de procesos son:
- Inicialización del sistema
- La petición de un usuario para la creación de un nuevo proceso. 
- Procesos que con ayuda de las llamadas al sistema se crea otro proceso.
Tenemos procesos en primer plano(foreground) qué interactuan con los usuarios y trabajan para ellos y procesos se segundo plano(background) qué no están asociados con un usuario sino que tienen alguna función específica, se los llama demonios en Unix y servicios en Windows. 
En Unix se crea un nuevo proceso a través de una llamada al sistema: **fork** qué de primera el nuevo proceso es un clon del proceso padre, mismas variables de entorno, imagen de memoria, etc., a continuación el proceso ejecuta execve o alguna llamada al sistema similar para cambiar su imagen de memoria y pasa a ejecutar un nuevo programa. 
