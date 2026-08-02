Las componentes que se involucran en la gestión de la memoria son. - el gestor de memoria del sistema operativo, el lenguaje de programación, el compilador, el montador y el hardware de gestión de memoria. 
Cada proceso requiere de una imagen de memoria, donde una imagen de memoria clásica de divide en Texto, Datos y Pila, pero puede haber información adicional para dar soporte a los hilos o a las bibliotecas dinámicas. 
- **Región de Texto**. Contiene el código maquina(métodos, funciones, lógica matemática, operadores). 
- **Región de Datos**. Lleva las variables globales incializadas, las no incializadas y datos creados dinámicamente(heap), en este espacio se crean las variables dinámicas. 
- **Region de Pila**. Soporta el entorno del proceso más los registros de activación de los procedimientos(*concepto del libro*). Se manejan pilas para poder mantener una gestion de las posibles necesidades de un proceso y se descartan una vez solventadas, rescatando lo necesario.
El gestor de memoria del sistema operativo es el encargado de cubrir las necesidades de memoria de los procesos, por lo que tiene encomendado las siguientes 2 funciones.- 
- Como servidor de memoria, se encarga de asignar espacios de memoria(intervalo de direcciones del mapa de memoria) aceptables para el proceso solicitante y una vez que no lo necesite, debe recuperar esos espacios. 
- Como gestor de recursos fisicos, debe gestionar la asignacion de memoria principal y de swap(memoria virtual).
## Jerarquia de almacenamiento
Los programas y datos deben estar presentes en la memoria principal(RAM) para poder ejecutar y ser referenciados, los no tan necesarios inmediatamente pueden estar en la memoria secundaria(disco). 
Mayormente las computadoras manejan 3 niveles de memoria, la memoria cache que es muy rapida, muy costosa y volatil, la memoria principal que es de mediana velocidad, mediano precio y volatil, y la memoria secundaria que es mas lenta, economica y no volatil.
Corresponde al sistema operativo coordinar el uso de estas memorias.
- **Memoria Caché**. Utilizada para compensar la enorme brecha de velocidad del procesador respecto a la velocidad de la memoria principal, sirve como un asistente personal del procesador, prediciendo qué es lo que necesitará a continuación, se cuenta con 3 niveles de Caché: 
- L1. Funciona en unos pocos nanosegundos y se divide en 2 partes. - L1 DC(Level 1 data cache), que almacena datos que son usados frecuentemente para reutilizarlos y L1 IC(Level 1 Instrucción cache), almacena instrucciones que son usadas frecuentemente para reutilizar después
- L2. Funciona a una velocidad de 11-14 ciclos de reloj(7ns) y se encarga de almacenar datos de uso frecuente. 
- L3. A una velocidad de 15-35ns se encarga de agilizar el acceso a datos e instrucciones que no fueron encontradas en L1 y L2, en caso de estar en L3, se busca en la RAM. 
- **Memoria RAM**.  Con una velocidad de acceso de 60-70ns.
- **SSD**.  Con una velocidad promedio de 50 microsegundos. 
- **Disco duro**.  Con una velocidad promedio de 8ms. 
La cache L2 utiliza gran parte del espacio del procesador porque requiere de millones de transistores para formar la Caché. Se crea utilizando SRAM(Static Random Access Memory) que para ser construido requiere por lo general 6 transistores por bit. Por otro lado la memoria RAM normal utiliza DRAM(Dinamic Random... )que puede crearse con un transistor por bit. 
## Administración de memoria
### **Monoprogramación** 
La forma más simple de administrar la memoria es ejecutando un programa a la vez, donde si requiere de otro, se retira el actual y se carga el nuevo programa. El sistema operativo puede residir en 3 posibles lugares. - en RAM, en ROM y en RAM con los controladores de dispositivos en ROM. 
### Multiprogramación 
Por un lado el sistema operativo se encarga de asignar y liberar memoria, por otro lado las bibliotecas de los lenguajes se encargan de asignar y recuperar dinámicamente memoria del heap. 
En general se dispone de un espacio de memoria contigua y de tamaño limitado sobre el que se realizan operaciones de asignación y liberación de memoria. Se tienen 3 forma de manejar la asignación dinámica de la memoria.
#### Particiones Fijas
Sistema de particiones fijas donde un espacio  disponible se divide a priori en partes o trozos de tamaño fijo, qué pueden ser del mismo tamaño o variar. Al momento de surgir una petición de memoria, se debe buscar una partición libre de tamaño adecuado a la petición. Debido a no saber cuanto espacio exacto necesitamos por cada petición de espacio, surge un derrochamiento de espacio que independientemente de la técnica para administrar la memoria siempre aparece, en el caso de las particiones fijas surge un derrochamiento llamado fragmentación interna. 
- **Fragmentación interna** es una parte del espacio asignado a un trabajo, que no está siendo empleada, si digamos tenemos una petición de n bytes y tenemos m bytes disponibles y m > n entonces tenemos m - n espacio sobrante, es decir fragmentación interna. 
- **Fragmentación externa** es aquella partición disponible donde su tamaño es insuficiente para almacenar una tarea. Es muy pequeña. 
#### Partición Variable
Al momento de asignar un espacio se genera normalmente un espacio libre, dependiendo del tamaño de las siguiente solicitudes podemos usar o no ese espacio libre, para el caso de no poder utilizar, debido a su pequeño tamaño, se produce una fragmentación externa. Ahora como remediamos con la colocación de almacenamiento tras una reserva de espacio disponible, se tienen 4 estrategias de colocación. - 
- First Fit. Es el más simple y lo que hace es almacenar en el primer espacio donde encuentre espacio suficiente para alojar el proceso. El S. O. revisa todas las secciones de memoria libre. 
- Next Fit. Intenta solucionar el problema del uso frecuente de los primeros espacios libres, entonces a partir del primer asignado busca un espacio libre suficiente. 
- Best Fit. Asigna al trabajo el espacio libre donde quede más justo, por ejemplo si tenemos 20, 15 y 10kb, y tenemos una petición de 7kb, se alojará en los 10kb. El algoritmo revisa toda la lista de espacios disponibles. 
- Worst Fit. Asigna el espacio más grande posible al trabajo que solicite. Este algoritmo suele dar la imagen de ser malo, como su nombre lo dice, pero existen situaciones donde tiene mejores resultados. 
