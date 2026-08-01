Las componentes que se involucran en la gestión de la memoria son. - el gestor de memoria del sistema operativo, el lenguaje de programación, el compilador, el montador y el hardware de gestión de memoria. 
Cada proceso requiere de una imagen de memoria, donde una imagen de memoria clásica de divide en Texto, Datos y Pila, pero puede haber información adicional para dar soporte a los hilos o a las bibliotecas dinámicas. 
- **Región de Texto**. Contiene el código maquina(métodos, funciones, lógica matemática, operadores). 
- **Región de Datos**. Lleva las variables globales incializadas, las no incializadas y datos creados dinámicamente(heap), en este espacio se crean las variables dinámicas. 
- **Region de Pila**. Soporta el entorno del proceso más los registros de activación de los procedimientos(*concepto del libro*). Se manejan pilas para poder mantener una gestion de las posibles necesidades de un proceso y se descartan una vez solventadas, rescatando lo necesario.
El gestor de memoria del sistema operativo es el encargado de cubrir las necesidades de memoria de los procesos, por lo que tiene encomendado las siguientes 2 funciones.- 
- Como servidor de memoria, se encarga de asignar espacios de memoria(intervalo de direcciones del mapa de memoria) aceptables para el proceso solicitante y una vez que no requiera, debe recuperar esos espacios. 
- Como gestor de recursos fisicos, debe gestionar la asignacion de memoria principal y de swap(memoria virtual).
## Jerarquia de almacenamiento
Los programas y datos deben estar presentes en la memoria principal(RAM) para poder ejecutarse y ser referenciados, los no tan necesarios inmediatamente pueden estar en la memoria secundaria(disco). 
Mayormente las computadoras manejan 3 niveles de memoria, la memoria cache que es muy rapida, muy costosa y volatil, la memoria principal que es de mediana velocidad, mediano precio y volatil, y la memoria secundaria que es mas lenta, economica y no volatil.
Corresponde al sistema operativo coordinar el uso de estas memorias.