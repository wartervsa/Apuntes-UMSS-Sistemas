## **Computador**
Un computador es una maquina que se encarga de procesar un flujo de datos a traves de instrucciones maquina y dar como resultado otro flujo de datos.
La computadoras se maneja bajo la arquitectura de Von Neumann que divide sus componenete en:
- Unidad aritmetico-logica.- Tiene la funcion de aplicar operaciones logicas y aritmeticas sobre 1 o 2 operandos. Lo datos se recogen de un banco de registros o de la memoria principal y se almacenan en un banco de registros o en la memoria principal. Tambien guarda informacion para el registro de estado que informa sobre si hay acarreo, negativo, es cero o hay desbordamiento, segun el valor del bit(0,1) se da un *salto condicional* que sirve para tomar decisiones en los programas.
- Memoria principal.- Se construye con memoria RAM(Random Acces Memory) y memoria ROM(Read Only Memory). Aca residen los datos a procesar, programas y resultados. La manera en que se transfiere informacion de la RAM al procesador y viceversa se hace a traves de una bus de datos, que limitado por el hardware de la placa base se envia la informacion con un tamano de palabra de 4 u 8 bytes, por lo cual se busca rellenar o siempre cumplir con un multiplo de 4.
- Unidad de entrada/salida.- Se encarga de la comunicacion entre la memoria principal y los perifericos.
- Unidad de control.- Es la encargada de hacer funcionar el conjunto, donde se realiza ciclicamente una secuencia:
	- Se lee de memoria la siguiente instruccion maquina que forma el programa.
	- Interpreta la instruccion leida: aritmetica, logica, etc.
	- Si hay datos referenciados en la instruccion, son leidos de la memoria.
	- Ejecuta la instruccion.
	- Si se tienen resultados de la instruccion, los guarda.
	La unidad de control tiene asociados una serie de registros, de los que cabe destacar:
	- Contador del programa(PC), indica la direccion de la siguiente instruccion maquina a ejecutar.
	- Puntero de Pila(SP), sirve para manejar comodamente una pila en memoria principal.
	- Registro de instruccion, que permite almacenar la instruccion maquina a ejecutar.
	- Registros de estados, que guarda informacion producida por las ultimas intrucciones maquina ejecutadas e informacion sobre como debe comportarse el computador.

**Modelo de programación del computador**

Tambien llamado arquitectura ISA(instructions Set Architecture), define los recurso y características que se ofrecen al programador de bajo nivel. Este modelo se caracteriza por los siguientes aspectos:

- Elementos de programación. - Son los elementos de almacenamiento visibles a las instrucciones máquina, de los que se incluyen los registros de estado, punteros de Pila, contador de programa, la memoria principal(ubi: mapa de memoria) y los registros de entrada/salida(ubi: registro de E/S)

- juegos de instrucciones.- Acá se encuentran todas las operaciones posible que puede hacer el computador. Con los modos de direccionamiento especifican la localización de los operandos.

- Secuencia de funcionamiento. - Definen el orden en que se van ejecutando las instrucciones.

- Modos de ejecución. - Parte importante de los computadores personales, donde se hace la diferenciacion entre modos de ejecución.

**Modos de ejecución**

Los computadores de propósito general mayormente presentan 2 o más modos de ejecución. Un modo menos permisivo, que está limitado en acciones, operaciones y accesos, como ciertas zonas del mapa de memoria, éste modo de ejecución generalmente se lo nombra modo usuario. Un modo más privilegiado, llamado modo privilegiado o núcleo, tiene total libertad para usar todas las operaciones posibles(como las operaciones de E/S) , acceder a todo el mapa de memoria, etc.

El computador arranca en modo privilegiado porque debe ejecutar el sistema operativo de primera, luego el S. O. se encarga de llevar a modo usuario todos los programas ejecutados por el usuario.

Se tienen un par de bits para determinar el modo de ejecución.

**Secuencia de funcionamiento del procesador**

La unidad de control del procesador es encarga de su propio funcionamiento. Repite una secuencia sencilla a muy alta velocidad. La secuencia la sigue ininterrumpidamente y en un bucle infinito.

- Lee de memoria principal la instrucción del contador del programa

- Incrementa el contador del programa.

- Ejecuta la instrucción.

El procesador solo sabe hacer esto muy velozmente por lo que se tienen mecanismos para dar más utilidad a estos pasos lineales, se tienen 3 mecanismos, pero los 3 lo logran de la misma manera, que es modificando el contador del programa.

- Instrucciones máquina de salto o bifurcación, pasa de una sección del programa a otra sección de si misma.

- Interrupciones, que modifican la unidad de control modifique el valor del contador del programa, lo que hace que salte a otro programa.

- Instrucción máquina de llamada al sistema, igual que interrupciones salta a otro programa.

## **Sistema operativo**

El objetivo de un computador de propósito general es el de ejecutar programas, por lo tanto el de un sistema operativo es el de facilitar la ejecución de esos programas. El S. O. es el intermediario entre el usuario del computador y su hardware, ofreciendo un entorno cómodo y eficiente.

Tiene como funciones clásicas:

- Gestionar los recursos del computador de manera eficiente, de forma que soporte con los requerimientos.

- Ofrece servicios a través de API's o llamadas al sistema.

- Una interfaz que permite al usuario dar órdenes al computador, se le atribuye el nombre de maquina extendida, que permite ejecutar programas al usuario evitando que se entere de la complejidad del hardware, ni lo que hace internamente.

Conceptualmente el sistema operativo se divide en 3 capas:

- Kernel(núcleo). - Se encarga de gestionar el hardware del sistema y parte de la funcionalidad básica del sistema operativo.

- Capa de servicios. - Son funcionalidades del computador que son usadas por los programas, por tanto es un apoyo a la elaboración de programas.

- Capa de intérprete de mandatos o shell, se encarga de la interacción del usuario con el computador, la shell recibe los mandatos del usuario, los interpreta y los ejecuta si es correcto.

**El sistema operativo como gestor de recursos**

Recurso es todo bien o medio necesario para conseguir lo que se pretende. En un computador tenemos recursos físicos(procesador, memoria... ) y recursos lógicos(temporizador, puerto de comunicaciones... ) estos recursos son limitados, usados por procesos que una vez ya no requieran de ellos son reutilizados por los demás procesos. Debido a que no se ejecuta un solo proceso y son varios a la vez, se debe tener un control de seguridad, qué no se acceda a recursos no permitidos, información sobre el uso que se hace de los recursos.

- Asignación de recursos.- Para asignar recursos a procesos, el sistema operativo mantiene estructuras que indican que recursos están libres y que proceso esta haciendo uso de que recurso/s. Se hadce la asignación según la disponibilidad y prioridad del proceso, ante peticiones de recursos coincidentes se deben resolver los conflictos.

- Protección. - Se evita que procesos accedan a recursos que ya están asignados a otros procesos para no comprometer la información de un proceso a otro.

- Contabilidad. - Registro de todos los recursos utilizados por un proceso durante su ejecución.

**Sistema operativo como máquina extendida**

Básicamente se proporcionan servicios o llamadas al sistema a los programas que son como complementos del modelo de programación, qué permiten ejecutar ciertas operaciones complejas de manera cómoda y eficiente. Se les puede dar una división como: Ejecución de programas, operaciones de E/S, detección de errores, servicios de memoria, operaciones entre procesos, etc.

**El sistema operativo como interfaz de usuario**

Permite al usuario dialogar con el intérprete de comandos o shell, el comportamiento de la shell se realiza en un bucle:

- La shell espera la orden del usuario, se puede tener una interfaz textual donde se requiere de conocimiento sobre la sintaxis y también se tiene la interfaz gráfica qué espera un click o alguna acción sobre ese programa.

- Interpreta el mandato y en caso de ser correcta lo ejecuta.

- Concluida la orden muestra un mensaje o prompt y regresa a esa espera.

## **Arranque del sistema**
