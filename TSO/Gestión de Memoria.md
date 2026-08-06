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
### **Multiprogramación** 
Por un lado el sistema operativo se encarga de asignar y liberar memoria, por otro lado las bibliotecas de los lenguajes se encargan de asignar y recuperar dinámicamente memoria del heap. 
En general se dispone de un espacio de memoria contigua y de tamaño limitado sobre el que se realizan operaciones de asignación y liberación de memoria. Se tienen 3 forma de manejar la asignación dinámica de la memoria.-
- Particiones fijas 
- Particiones variables
- Sistema buddy binario
#### Particiones Fijas
Sistema de particiones fijas donde un espacio disponible se divide a priori en partes o trozos de tamaño fijo, qué pueden ser del mismo tamaño o variar. Al momento de surgir una petición de memoria, se debe buscar una partición libre de tamaño adecuado a la petición. Debido a no saber cuanto espacio exacto necesitamos por cada petición de espacio, surge un derrochamiento de espacio que independientemente de la técnica para administrar la memoria siempre aparece, en el caso de las particiones fijas surge un derrochamiento llamado fragmentación interna. 
- **Fragmentación interna** es una parte del espacio asignado a un trabajo, que no está siendo empleada, si digamos tenemos una petición de n bytes y tenemos m bytes disponibles y m > n entonces tenemos m - n espacio sobrante, es decir fragmentación interna. 
- **Fragmentación externa** es aquella partición disponible donde su tamaño es insuficiente para almacenar una tarea. Es muy pequeña. 
#### Partición Variable
En  la particion variable se asigna la memoria solicitada en un espacio libre, al momento de asignar un espacio se genera normalmente un espacio libre, dependiendo del tamaño de las siguiente solicitudes podemos usar o no ese espacio libre, para el caso de no poder utilizar, debido a su pequeño tamaño, se produce una fragmentación externa. Ahora como remediamos con la colocación de almacenamiento tras una reserva de espacio disponible, se tienen 4 estrategias basicas de colocación. - 
- **First Fit.** Es el más simple y lo que hace es almacenar en el primer espacio donde encuentre espacio suficiente para alojar el proceso. El S. O. revisa todas las secciones de memoria libre. 
- **Next Fit**. Intenta solucionar el problema del uso frecuente de los primeros espacios libres, empezando del primero que fue asignado y buscando un espacio libre suficiente. 
- **Best Fit**. Asigna al trabajo el espacio libre donde quede más justo, por ejemplo si tenemos 20, 15 y 10kb, y tenemos una petición de 7kb, se alojará en los 10kb. El algoritmo revisa toda la lista de espacios disponibles. 
- **Worst Fit**. Asigna el espacio más grande posible al trabajo que solicite. Este algoritmo suele dar la imagen de ser malo, como su nombre lo dice, pero existen situaciones donde tiene mejores resultados que los demas.
Para poder gestionar el espacio de almacenamiento, debemos llevar un registro de los bloques y huecos existentes, con el fin de facilitar la asignacion y liberacion de espacio. Suele manejarse una lista doblemente encadenada que contiene los huecos disponibles. El orden de la esta lista suele hacerse de 3 maneras.- por LIFO, por el tamano de los huecos o por la direccion de comienzo de cada hueco.
Para solucionar el problema de la fragmentacion externa se aplica una compactacion, la cual consiste en juntar los huecos libres formando un hueco mas grande. Esto por supuesto trae consigo un coste de operacion alto y dificil de realizar, ademas el s.o. debe detener todo para realizar la compactacion.
Podemos agregar una forma de manejar la memoria de los objetos de un programa que ya no los utiliza, con un recolector de basura(garbage collector), aquel esta implementando en algunos lenguajes de programacion.
#### Sistema buddy binario
Consiste en partir a la mitad una particion disponible hasta que se ajuste a la solicitud. Si tenemos un hueco de M bytes, aquel lo podemos dividir en M/2, M/4, M/8, etc. Al momento de solicitar espacio, divide hasta que lo solicitado se ajuste a la division. Esto provova una fragmentacion interna que se trata de solventar uniendo con otra mitad correspondiente para formar un hueco que sea M/lo que se libero + el equivalente. Por ejemplo si tenemos 256kib y se solicitan 20kib entonces dividimos los 256 en 128, luego 64, 32 y continuaria con 16 pero como no es suficiente espacio se le asignan los 32kib, teniendo una frag. interna de 12kib. Fuentes externas al libro mencionan que debe ser un espacio disponible que sea potencia de 2, osea siempre manejamos un 2^n espacios disponibles, con el fin de agilizar las asignaciones con operaciones binarias a nivel de bits.

## **Esquema de gestion de la memoria**

Para esquematizar la gestion de la memoria analizaremos como se reparte la memoria del sistema entre los distintos procesos que estan ejecutandose en un momento dado. 
Memoria real.-
- Asignacion contigua.
- Segmentacion.
Memoria virtual.-
- Paginacion.
- Segmentacion paginada.
```
*Aclaracion necesaria, la memoria real se refiere exclusivamente a la RAM, es algo tangible, fisico, pero no se incluye la ROM ni el disco de almanacenamiento(Se considerean memoria secundaria).
La memoria virtual es una abstraccion de esa memoria, es la forma en que el SO presenta a los programas como visualizar la memoria*
```
Los sistemas operativos actuales de propósito general utilizan exclusivamente gestión de memoria basada en memoria virtual.
#### Asignacion contigua
Aqui los procesos son asignados a una unica zona contigua de la memoria principal, delimitada por un tamano o una direccion del final.
#### Segmentacion
La segmentacion es una extension de la asginacion contigua, donde asigna a los procesos zonas o segmentos de memoria principal contigua. La manera en que se maneja fisicamente es unidimensional, el sistema solo ve un arreglo de dividido en paginas, mientras que el programador mira subrutinas o estructuras de datos por su nombre. La segmentacion es una manera de administrar la memoria que ayuda al usuario a visualizar la memoria como un conjunto de segmentos, cada uno con su nombre y tamano(el cual puede variar dinamicamente). De esto surgen las **direcciones logicas** compuestas por un numero de segmento y desplazamiento, para traducir de la direccion logica a una direccion fisica tenemos una tabla de segmentos. La **tabla de segmentos** contiene la base de donde comienza fisicamente ese segmento y el tamano fisico del segmento.
Una ventaja de la segmentacion es la posibilidad de proteccion por segmento, puesto que cada segmento puede ser dato o instruccion se le puede dar un permiso apropiado.
La segmentación también facilita la compartición de código o datos entre varios procesos.

#### Paginacion 
Este esquema es el mas usado por los sistemas operativos actualues, macOS, Linux y Windows.
La paginacion viene a confrontar un problema que surge de la segmentacion. Sucede que aquel produce fragmentacion externa que es dificil de conllevar, por lo cual la paginacion implica la division de la memoria fisica en bloques de tamano fijo(***marco***) y ademas dividir la memoria logica en ***paginas*** del mismo tamano. 
Se utiliza, con apoyo del hardware, una tabla de paginas que relaciona una direccion logica(nro. de pagina, desplazamiento(offset)) con la direccion fisica(***marco***), el tamano de las paginas es una potencia de 2.
Se tiene un componente llamado TLB(Traslation Look-aside Buffer) que contiene las parejas pagina-marco de las paginas que han sido recientemente usadas, renovando la informacion de la TLB con valores interes en cada momento. Es una cache especializada para la tabla de paginas que contiene valores ordenados de 64 a 1024 entradas. La MMU, que es el traductor de direcciones logicas a fisicas, primero accede a la TLB antes que a la memoria real, solamente tras un fallo(que no lo encuentra) de la TLB, es que recurre a la memoria principal y sustituye alguna pareja pagina-marco con la nueva informacion.
El sistema operativo determina las paginas virtual de un programa en ejecucion y su soporte fisico, ya sea con un marco o una pagina de intercambio.DIcha informacion es reflejada en la tabla de paginas, ques soportada y construida por le s.o.
Tomando en cuenta que el sistema operativo es un programa en ejecucion al igual que el programa de un usuario, se tendria que hacer un cambio constante entre el sistema operativo y el programa del usuario, por esto se maneja un pieza fisica como extension del procesador encargada de traducir la direccion logica a fisica. Dicho hardware especial es llamado MMU(Memory Management Unit).
Cada proceso tiene su propio mapa de memoria y su propia tabla de páginas que define dicho mapa, cuando la CPU se concede a otro proceso, hay que cambiar la tabla de páginas a la del nuevo proceso. La paginación en general encarece los cambios de contexto. La seguridad sale gratis, ya que cada proceso sólo puede accesar las páginas que están en su tabla de páginas.
Existe un registro para indicar a la MMU la direccion de memoria donde se encuentra la tabla de paginas, dicho registro se denomina RIED(Registro Identificador del Espacio de Direccionamiento).
**Que ocurre si el tamano de la pagina es muy pequeno?**
Estariamos degradando la velocidad de acceso a las paginas tanto de la TLB como de la RAM, por un exceso de indices lo cual ocupa ademas mas espacio.
**Que ocurre si el tamano de la pagina es muy grande?**
Se produce mucha fragmentacion interna.
A dia de hoy, en 2026, los sistemas operativos habilitan una tecnologia llamada Huge Pages(paginas gigantes, que permite manejar paginas mas grandes para ciertos programas que se vea conveniente.


