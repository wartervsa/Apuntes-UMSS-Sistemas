En las computadoras actuales se manejan varios procesos a la vez, aunque estrictamente un procesador solo maneja un proceso a la vez, pero lo hace tan rápido, donde pudo cambiar entre programas en 1 segundo, que da la sensación de paralelismo(pseudoparalelismo). Ahora ésta rápida conmutación de un proceso a otro en algún orden se llama multiprogramación. 
**Un programa** es una entidad estática constituida por sentencias de programa que definen el comportamiento de los procesos cuando se ejecutan utilizando un conjunto de datos. 
**Un proceso** es la ejecución del programa, es una instancia del programa con un conjunto de datos. 
Como por ejemplo sería como en programación orientada a objetos donde la clase sería el programa y los objetos serían los procesos. 
### **Creación de un proceso**
Los sistemas operativos deben asegurarse, de alguna manera, que puedan existir todos los procesos que sean requeridos. El so está encargado de la ejecución y terminación de procesos mientras esté operando. De los sucesos principales para la creación de procesos son:
- Inicialización del sistema
- La petición de un usuario para la creación de un nuevo proceso. 
- Procesos que con ayuda de las llamadas al sistema se crea otro proceso.
Tenemos procesos en primer plano(foreground) qué interactuan con los usuarios y trabajan para ellos y procesos se segundo plano(background) qué no están asociados con un usuario sino que tienen alguna función específica, se los llama demonios en Unix y servicios en Windows. 
En Unix se crea un nuevo proceso a través de una llamada al sistema: ***fork*** qué de primera el nuevo proceso es un clon del proceso padre, mismas variables de entorno, imagen de memoria, descriptores de archivo, etc., a continuación el proceso ejecuta ***execve*** o alguna llamada al sistema similar para cambiar su imagen de memoria y pasar a ejecutar un nuevo programa. 
En Windows con una única llamada al sistema de Win32, ***CreateProcess*** se realiza la creación del proceso y del programa. En ambos so los procesos hijo tienen espacios de dirección disjuntos. 
### **Terminación de procesos**
Evidentemente tarde o temprano se concluirá la tarea encomendada al proceso, entonces éste debe terminar, algunas de las razones serían:
- El proceso finaliza su tarea y termina(volutario). 
- El proceso encuentra algún tipo de fallo y termina(voluntario). 
- El sistema detecta un error en el proceso y lo termina. 
- Otro proceso termina con otro proceso. 
Un proceso puede terminar voluntariamente, porque finalizó con su tarea. En Unix tenemos ***exit*** y en Windows ***ExitProcess***. En programas orientados a la pantalla(una interfaz gráfica) se tiene una opción para terminar con el proceso voluntariamente. 
Para dar una mejor idea del tercer punto mencionado, puede darse el caso de que el programa quiera ejecutar una sentencia de programa ilegal, que sea incorrecto, entre otros. Para el cuarto punto de que un proceso quiera forzar la terminación de otro proceso puede usarse ***kill*** o ***killall*** en Unix, en Windows ***TerminateProcess***, en ambos casos el proceso "asesino" debe tener la autorización necesaria. 
### **Estados de un proceso**
Se tienen unos cuantos estados qué corresponde a un determinado tiempo o evento. 
- Nuevo(new)
- Ejecutandose(running), está siendo ejecutado por la CPU, que hablando de un uniprocesador solo puede estar 1 proceso en este estado. 
- Listo para ejecutar(ready), tiene todo preparado para ejecutarse y espera su turno de CPU. 
- Bloqueado/en espera/suspendido(waiting), no está en condiciones para ejecutarse. Espera a que algún evento ocurra, como esperar alguna operación de E/S. 
- Terminado(terminated)
Se puede tener una cola de listos en orden prioritario y una cola de bloqueados o en espera desordenada. 
El sistema operativo tiene un planificador(scheduler) como parte de su núcleo, y su misión es seleccionar el proceso a ejecutar a continuación. También tenemos al despachador(dispatcher) qué se encarga de asignar el primero proceso en la cola de listos, seleccionada por el scheduler, a ser ejecutado por la CPU. 
Ahora hacer estos cambios entre procesos requiere de preservar lo ya avanzado en uno u otro proceso, sino de que manera se mantendría la consistencia de los procesos. Debido a esto se tiene un registro de datos necesarios para su implementación. La PCB o bloque de control de proceso(Procces Control Block) mantiene la información necesaria para reanudar la ejecución del proceso, como por ejemplo:
- Información de su estado(listo, detenido, en espera, etc.)
- Registros de CPU, como el contador de programa, punteros de Pila, registros índice, entre otros que varían entre arquitecturas del computador. 
- Información de planificación, como prioridad, punteros a colas de planificación y cualquier otro parámetro de planificación. 
- Información para la administración de memoria, como pueden ser las direcciones logicas, registros de base y limite, tabla de segmentacion o tabla de páginas. 
- Información de estado de E/S(ej. : archivos abiertos, acciones, etc. )
- Estadísticas y otros(ej. : tipo de CPU usado, id. del proceso, id. del dueño, etc. )
Al estar guardando la información de los procesos constantemente e intercambiarse para ser ejecutado, deriva a un costo relativamente alto de procesamiento, donde la CPU no avanza con ningún proceso, a esta acción se le denomina Context Switch(Cambio de contexto). Para reducir este problema se emplean estructuras qué se mencionaran más adelante(hilos). 
### **Jerarquía de procesos**
La secuencia de creación de procesos genera una jerarquía de procesos, cual árbol genealógico, donde se tiene un proceso padre y un proceso hijo. 
En Unix mantienen de forma explícita su jerarquía partiendo del único que no tiene padre llamado ***init***(actualmente en Linux se tiene ***systemd***) qué se encarga de leer de un archivo las terminales que se tienen y crea un proceso ***login*** por cada una de ellas, luego el mismo se encarga del inicio de sesión correcto del usuario para asignarle una ***shell*** qué se encarga de crear los procesos del usuario, así generando un árbol de procesos. 
En Windows si bien no se mantiene una jerarquía, se puede hacer una analógia de sus procesos iniciales, uno de los primeros es el ***System*** qué deriva a una sesión con el proceso ***Winlogon***, seguido de la creación de su shell ***Explorer***, y apartir de acá se crean todos los procesos de usuario como procesos hijos de el.
## **Procesos livianos, Hilos, Hebras o Threads**
Cada proceso tiene su propio espacio de direcciones y un unico flujo(hilo) de control, pero muchas veces es necesario tener mas de un flujo de trabajo, por la cantidad que vean necesaria los desarrolladores(ej.: por funcionalidades, por velocidad de respuesta o por aprovechamiento de varios nucleos). Estos hilos pertenecen a un proceso donde comparten el mismo espacio de direcciones y recursos asignados, cada hilo funciona como unidad de computacion por lo que se tiene un contador de programa, stack y un conjunto de registros para su ejecucion. Entonces tenemos un proceso pesado que contiene toda la informacion necesaria(PCB) para la vida del mismo y tenemos procesos livianos(hilos) que contienen lo necesario para ejecutar una actividad/funcionalidad/unidad de computo. La informacion de estos hilos al ser reducida ayuda con el costo de los context switch.
Respecto a la implementacion tenemos la vision mas conceptual donde se divide la PCB(del proceso) y la TCB(del hilo), en la practica, por ejemplo en Linux, no se distingue entre esas 2 estructuras, sino se hace la diferencia por los recursos compartidos. En Windows la unidad basica son los procesos livianos, por lo cual se trabaj sobre esta.
## **Planificacion(scheduling) de procesos**
En un ambiente de multiprogramacion se tendran momentos donde 2 o mas procesos compiten(se encuentra listos simultaneamente) por la CPU, es entonces que se debe poder escoger cual se ejecutara a continuacion, el que se encarga de esto es el planificador(scheduling) y hace la eleccion bajo un algoritmo de planificacion. Un algoritmo de planificacion debe cumplir criterios basicos como.- 
- Maximizar el uso de la CPU -> el procesador se usa el mayor tiempo posible.
- Maximizar la productividad -> ejecutar la mayor cantidad de procesos por unidad de tiempo.
- Minimizar el tiempo de retorno -> tiempo transcurrido desde que empieza hasta que termina.
- Minimizar el tiempo de espera -> tiempo que el proceso esta en la cola de espera.
- Minimizar el tiempo de respuesta -> tiempo que transcurre desde la solicitud hasta al respuesta, sin contar la respuesta en si.
- Equidad -> Todos lo procesos deben ser ejecutados por igual.
- Recursos equilibrados -> que se matengan ocupados los recursos del sistema, pero sin que abusen los procesos de los recursos asignados, degradando el performance general.
Se manejan diferentes colas para la organizacion de los procesos para un uso adecuado de los recursos. Se tiene una cola de dispositivos(device queues), al ser creado un proceso se coloca en la cola de trabajos, donde residen procesos listos para ejecutar pero que no se encuentran en memoria principal y tambien se tiene una cola de listos(ready queue) que contiene los procesos listo para su ejecucion y se encuentra en la memoria principal.
Los elementos que almacena una cola de listos son las PCB's de los procesos. 
**Reloj de interrupcion o Temporizador de intervalos**
El sistema operativo tiene un reloj que sirve principalmente para interrumpir al procesador para comprobar si ya finalizo la tarea y tambien para controlar que no pase un tiempo establecido de ejecucion por proceso.
### **Algoritmos de planificacion**
Dependediendo de los objetivos de un sistema operativo se puede hacer una diferenciacion entre 3 maneras de que un proceso llegue a la cola de listos(ready queue):
- Batch(lotes).- Llega un grupo de procesos y se decide sobre ese grupo.
- Interactivo.- De acuerdo al requerimiento de una aplicacion(ej.: escribir con el teclado).
- Tiempo Real.- Llega todo el tiempo.
Un proceso mientras no finalice, obviamente si haber terminado, vuelve a la cola de listo por uno de los siguiente motivos.- 
- Por peticion de E/S.
- Su tiempo de ejecucion asignado expira.
- Al ejecutar otro proceso(su hijo).
- Ocurre una interrupcion(ej.: mover el cursor, presionar una tecla).
 **Niveles de planificacion del procesador**
 - Planificacion de nivel alto(largo plazo).- Tambien denominado planificacion de admision, determina que trabajos estan permitidos para competir activamente por recursos del sistema.
 - Planificacion de nivel intermedio(mediano plazo).- Determina que procesos estan permitidos para competir por la CPU, suspende o reanuda procesos.
 - Planificacion de bajo nivel(corto plazo).- Determina que proceso listo sera asignado a la CPU cuando esta queda disponible y es asignado al CPU(scheduling y dispatcher).
 **Planificación Apropiativa y No Apropiativa**
La planificación no apropiativa es aquella que una vez se da la CPU a un proceso, éste se ejecuta hasta terminar o hasta que ocurra un determinado evento. 
La planificación apropiativa sucede que la CPU está ejecutando un proceso pero puede ser interrumpido y llevado a la cola de listos por decisión del sistema operativo. Si bien los no apropiativos requieren un mayor costo, se ve beneficiado el sistema al evitar que un proceso se apropie de la CPU. Para estos cambios el sistema operativo debe ser capaz de diferenciar procesos criticos, por ejemplo una llamada al sistema(system call), la escritura de un archivo, entre otros. 
- FCFS o FIFO son no apropiativos, porque capturan la CPU hasta terminar o requieren E/S. 
- SJF(Shortest-job-first) es apropiativo y no, si es apropiativo ocurre cuando llega un proceso cuyo tiempo es menor que el del activo, el sistema operativo puede tomar la decisión de interrumpir el que se está ejecutando o no. 
- Por Prioridades puede apropiativo o no, cuando es apropiativo puede haber llegado un proceso con mayor prioridad y es interrumpido. En no apropiativo si llega un proceso de mayor prioridad no es interrumpido pero se lo llevo al frente de la cola. 
- Round Robin es apropiativo, porque se le da un tiempo máximo de ejecución a cada proceso y una vez que llega es interrumpido.
**Planificación de procesos en Linux(POSIX) y Windows(Win32)**
En Windows dicen manejar solo hilos pero se puede comprobar que también usan procesos pesados. Se manejan dos clases de planificación, Prioridad de Tiempo Real(16 a 31) y Prioridad Dinámica(1 a 15). Internamente se maneja RR y se puede hacer la diferenciación entre los procesos del sistema, que no puede ser modificado su prioridad y procesos del usuario, que es posible cambiar su prioridad. Si llega un thread de mayor prioriodad, el procesador es asignado a este. La prioridad va de menor a mayor y la prioridad de un hilo no puede ser modificada, pero si del proceso pesado.
En Linux se usan 3 tipos de clases con prioridades.- Normal(100-139), RR fija(0-99) y FIFO fija(0-99), aca las prioridades bajas son favorecidas y podemos cambiar practicamente las prioridades de todo.