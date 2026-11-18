Es un sistema tenemos un numero de recursos limitados, estos recursos pueden ser:
- **Fisicos**.- Ciclos de CPU, espacio en memoria, dispositivos de E/S(impresora, teclado, mouse, disco duro, etc.)
- **Logicos**.- Archivos, tablas del sistema, registros de base de datos, semaforos, entre otros.
Estos son asignados a procesos que solicitan una cantidad de recursos, dicha solicitud no debe exceder la cantidad de recursos existentes. Cada proceso debe solicitar esos recursos antes de usarlo y debe liberar al terminar de usar, un proceso debe solicitar lo recursos necesarios para la llevar a cabo su tarea. Surgen momentos donde ciertos recursos no estan disponibles en el momento de la solicitud y deben esperar a que el proceso que los tiene asignados libere esos recursos, pero puede ocurrir que tambien este esperando recursos,y que el proceso que los tiene tambien y asi sucesivamente, esto genera un bloqueo.
Los recursos son de 2 tipos:
- **Recursos Apropiativos**.- Son aquellos que pueden ser tomados del proceso que los posee sin producir un dano.
- **Recursos no Apropiativos**.- Son aquellos que no pueden ser tomados del proceso que los posee porque podria producir un fallo de calculo. En general los bloqueos se relacionan con recursos no apropiativos.
Se tiene una secuencia de eventos necesarios para utilizar los recursos:
- Solicitar el recurso, si el recurso solicitado no puede ser asignado inmendiatamente, el solicitante debe esperar.
- Utilizar el recurso.
- Liberar el recurso
La solicitud y la liberacion de recursos son peticiones al sistema, tambien el uso de los recursos puede ser a traves de las llamadas al sistemas. Por lo tanto el sistema operativo esta al tanto de que recursos fueron pedidos por un proceso o que recursos estan asignados a que procesos, esto se maneja a traves de una tabla y tambien se tiene, para los procesos que solicitan un recurso que esta ocupado, una cola de procesos para ese recurso.
## **Bloqueos**
Un conjunto de procesos esta en estado de interbloqueo cuando cada uno de ellos
espera un suceso que sólo puede originar otro proceso del mismo conjunto. Los procesos permanecen bloqueados para siempre, a esta situacion le llama **bloqueo**, un ejemplo.-
![[Pasted image 20260721181242.png]]
En ambientes de multiprogramacion, varios procesos pueden competir por un numero finito de recursos. Cuando un proceso solicita recursos que estan ya asignados, el proceso entra en estado de espera, si el otro proceso que tiene los recursos los tiene detenidos es entonces que entra el solicitante en un estado de espera que nunca cambia, a esto se le llama *abrazo mortal*(Deadlock, interbloqueo o bloqueo mutuo). 
Para evitar este problema el sistema toma una decision relativamente drastica, que podria ser tomar los recursos de los procesos que estan en interbloqueo.

## Condiciones de interbloqueo
Segun Coffman en su libro ***"System deadlocks"*** menciona cuatro condiciones necesarias para que ocurra un interbloqueo o bloqueo, estas cuatro condiciones deben presentarse simultaneamente para producirlo.
- **Exclusion mutua**. Los recursos pueden estar asignados unicamente a un proceso o estar disponible. Un recurso esta asignado precisamente a un unico proceso, si otro solicita, queda esperando hasta ser liberado.
- **No expropiacion**. Los recursos ya asignados no pueden ser desasignados, debe ser el mismo proceso el que lo haga voluntariamente o una vez termine su tarea.
- **Espera circular**. Debe haber una cadena de 2 o mas procesos que entre todos esperen un recurso asignado al siguiente miembro de la cadena.
- **Retener y esperar**. Los procesos que tengan asignado un recurso y ademas soliciten recursos adicionales que estan ya asginados a otros procesos, mientras esperan deben retener los recursos que tienen.

## **Modelacion de interbloqueos**
Los interbloqueos se pueden representar en un grafo dirigido. Tenemos 2 tipos de nodos(N): recursos y procesos, tambien se tienen arcos(A) que implican lo siguiente: si un proceso apunta al recurso, hablamos de un proceso que solicita un recurso y para un recurso que apunta un proceso, hablamos de un recurso asignado al proceso apuntado.
![[Pasted image 20260726182331.png]]

## **Métodos para manejar interbloqueos**  
Podemos dividir en 2 tipos de maneras de lidiar con los interbloqueos:  
### **Ignorar el problema**  
La más simple de todas es el algoritmo avestruz o de Ostrich, qué consiste en ignorar el problema. Los S. O. que manejan este método parten de la hipótesis de que los usuarios preferirían bloqueos ocasionales y continuar con el uso los demás distintos recursos. Claro que se tienen problemas con éste método como en las restricciones de los procesos y en el uso de recursos.  
### **Afrontar el problema**  
Se manejan 4 métodos donde 2 de ellos aseguran con un protocolo que nunca se entrará en un abrazo mortal y otro donde se permite que ocurra un abrazo mortal pero se recupere después, claro que recuperar es más difícil y caro.  

#### **Prevención de bloqueos**  
A grandes rasgos consiste en diseñar un sistema que excluya, a priorio, la posibilidad de interbloqueo. Se consigue asegurandonos de preveer qué ocurra una de las 4 condiciones de Coffman.  
- **Exclusión mutua**. Se tienes recursos que podrían ser manejados sin exclusión mutua, por ejemplo una impresora usando spooling, acá se hace uso de otro proceso que manejaria una cola de las impresiones, un proceso A y un proceso B pueden al mismo tiempo enviar a la cola de impresión y el spooler se encargará de imprimir ordenadamente, así preveemos el interbloqueo. Ocurre que hay recursos del sistema que no pueden permitirse este tipo de técnica, por ejemplo una tabla de procesos, transacciones, semáforos, qué deben tener exclusión mutua obligatoriamente y por lo cual debe buscar una prevención en las otras 3 condiciones.
- **Retener y esperar**. Para preveer un interbloqueos en la condicion de retener y esperar debemos asegurarnos de que el proceso que solicite algun recurso ya no tenga alguno asignado. El primer protocolo propone asignar al procesos todos los recursos necesarios antes de que su ejecucion, se podria lograr haciendo que las llamadas al sistema de asignacion de recursos suceda antes que las demas llamadas al sistema. El segundo protocolo propone que cada vez que el proceso no retenga algun recurso, recien es posible que solicite otro recurso. Para ambos protocolos surgen desventajas, como el poco uso de el conjunto de recursos necesarios, porque no seran utilizados la mayor parte del tiempo debido a que el ultimo recurso del conjunto se usara unicamente al final de todo pero estaria siendo ocupado todo ese tiempo. Una segunda desventaja aparece cuando un proceso trata de solicitar recursos muy solicitados, lo que podria generar inanicion.
- **No expropiacion**. Con el fin de asegurar que esta condicion no se cumpla, se propone el siguiente protocolo.- Aquel proceso que solicite recursos adicionales y esos recursos esten asignado a otro proceso, osea que debe esperar, entra a su lista de recursos necesarios, tanto los asignados como los solicitados. Ahora los procesos que soliciten otros recursos adicionales y veamos que pertenece a la lista de los recursos necesarios de otro proceso, teniendo obviamente como asignado el solicitado, es entonces que se los desasigna y asigna al solicitante. Un proceso puede ser reinicializado si consigue el recurso solicitado y alguno de los que tenia asignado. Dicho protocolo es usado para recursos del cual su estado es facilmente recuperable, por ejemplo los registros del CPU y el espacio en memoria. Por otro lado se tienen recursos como la impresora, que si fuera aplicado aqui, se tendrian resultados confusos.
- **Espera circular**. Se propone una solucion para preveer la espera circular, en la cual se asignaria un numero del conjunto de numero naturales a cada recurso de un conjunto de recursos y se haria una comparacion de que un recurso sea menor o mayor para poder ser asignado. Lo que deriva a la regla de que los procesos pueden solicitar recursos siempre y cuando devuelva el recurso que tenga y el que solicite sea mayor al que tiene. Basicamente estariamos generando un orden.

#### **Evitacion de interbloqueos**
Para evitar un interbloqueo podemos, antes de centrarnos en la forma o manera de solicitar un recurso, verificar si es seguro asignar esos recursos. Si se presentan las condiciones necesarias para un interbloqueo, podemos colocar una restricción que evite una de las condiciones de interbloqueo, llevando a un estado seguro. Otra manera de evitar interbloqueos es adicionando información a la solicitud de recursos, como la necesidad máxima de recursos de los procesos que están en ejecución, la asignación actual de recursos a procesos y la cantidad actual de instancias libre de casa recurso. Con esta información podríamos predecir posibles interbloqueos y asegurarnos de que nunca ocurran. 
**Estados seguros e inseguros**
- Un estado de asignación de recursos es *seguro* si en el no hay posibilidad de interbloqueo. Para lograr un estado seguro se debe conseguir una secuencia segura, que se consigue ordenando los procesos de modo que puedan ser asignados a los procesos los recursos disponibles(libres) más los retenidos por los demás procesos que finalizaron, repitiendo el mismo proceso en cada solicitud. 
- Un estado *inseguro* es aquella situación donde se presenta la posibilidad de interbloqueo. 
#### **Detección de bloqueos**
Los anteriores métodos de evasión o prevención debían estar constantemente verificando en cada solicitud si llegaba a un estado seguro. Lo que se aplica en la detección es una verificación de circularidad cada que sea apropiado(hora, día o se perciba rendimiento reducido). Para comprender la idea se puede representar mediante los grafos, donde debemos de asegurar que ocurra una de las siguientes. - 
1. Si todos los procesos pueden terminar, se concluye que no existe interbloqueo. 
2. Si no pueden terminar entonces se concluye que existe un interbloqueo entre los que no terminaron. 
Para desaparecer este interbloqueo, se captura los recursos necesarios del proceso que los tiene, se termina con ese proceso y se liberan sus recursos. 
#### **Recuperación de Bloqueos**
Una vez detectado el posible interbloqueos debemos desarmarlo y recuperar el sistema lo más rápido posible, existen varios métodos, pero tienen una característica común: elegir una **victima**, trabajo que al ser eliminado, libere al sistema del interbloqueo. 
- El sistema mata todos los procesos actuales y vuelve a arrancar. Básicamente un reinicio forzado. 
- Terminar los procesos involucrados en el interbloqueos y solicitar al usuario arrancarlos de nuevo. 
- Identificar uno por uno lo procesos involucrados en el interbloqueo y terminar uno por uno con ellos hasta desaparecer el interbloqueo, así los procesos restantes terminan su trabajo y los eliminados volveran a iniciar desde el principio. 
- Para realizar éste método, el trabajo a eliminar debe tener un registro de su progreso, de manera que se pueda interrumpir y después continuar sin necesidad de reiniciar desde el principio. 
- Se selecciona un trabajo no bloqueado para retirar sus recursos y asignar esos recursos a procesos bloqueados.
- Se detiene a los procesos nuevos e impide qué ingresen al sistema. Se hace con la idea de que los procesos actuales bloqueados logren terminar su ejecución y que no hay mucha competencia por los recursos. Es con suerte posible que obtengan los recursos necesarios. Único método que no requiere víctima. 
Para seleccionar el trabajo víctima se deben considerar algunos aspectos. - 
- La prioridad del trabajo en consideración. 
- El tiempo de CPU utilizado por el trabajo. No se suelen tocar los trabajo a punto de terminar. 
- La cantidad de tareas que repercutiria la elección de la víctima. 
- Los programas que trabajan con bases de datos, aunque muchos sistemas de bases de datos emplean métodos de recuperación. 
**¿Qué se usa en Windows, Linux y Macos actualmente?**
Básicamente ignoran el problema y ceden al usuario la búsqueda de una solución, puede parecer que parte de la pereza de los programadores, pero el motivo es muy convincente y lo resumire en:
- **Cálculo complejo matemático**. Algoritmos como el *Algoritmo del banquero de Dijkstra*  requieren de un cálculo antes de la asignación de los recursos, lo que causa que el procesador pase más tiempo haciendo el cálculo que ejecutando código útil. 
- No es posible saber a ciencia cierta qué recursos serán necesarios para un proceso. 
Con ésta cuestión quedará más claro ¿prefieres que tu computadora vaya un 40% más lento? Evitando cualquier tipo de bloqueo, ó prefieres tener ese rendimiento y que solo una vez al mes, posiblemente, ocurra un bloqueo, además que lo puedes matar sin mucho poblema. 