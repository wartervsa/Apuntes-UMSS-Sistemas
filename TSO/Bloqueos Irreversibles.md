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
### **Bloqueos**
Un conjunto de procesos esta en estado de interbloqueo cuando cada uno de ellos
espera un suceso que sólo puede originar otro proceso del mismo conjunto. Los procesos permanecen bloqueados para siempre, a esta situacion le llama **bloqueo**, un ejemplo.-
![[Pasted image 20260721181242.png]]
En ambientes de multiprogramacion, varios procesos pueden competir por un numero finito de recursos. Cuando un proceso solicita recursos que estan ya asignados, el proceso entra en estado de espera, si el otro proceso que tiene los recursos los tiene detenidos es entonces que entra el solicitante en un estado de espera que nunca cambia, a esto se le llama *abrazo mortal*(Deadlock, interbloqueo o bloqueo mutuo). 
Para evitar este problema el sistema toma una decision relativamente drastica, que podria ser tomar los recursos de los procesos que estan en interbloqueo.

### Condiciones de interbloqueo
Segun Coffman en su libro ***"System deadlocks"*** menciona cuatro condiciones necesarias para que ocurra un interbloqueo o bloqueo, estas cuatro condiciones deben presentarse simultaneamente para producirlo.
- **Exclusion mutua**. Los recursos pueden estar asignados unicamente a un proceso o estar disponible. Un recurso esta asignado precisamente a un unico proceso, si otro solicita, queda esperando hasta ser liberado.
- **No expropiacion**. Los recursos ya asignados no pueden ser desasignados, debe ser el mismo proceso el que lo haga voluntariamente o una vez termine su tarea.
- **Espera circular**. Debe haber una cadena de 2 o mas procesos que entre todos esperen un recurso asignado al siguiente miembro de la cadena.
- **Retener y esperar**. Los procesos que tengan asignado un recurso y ademas soliciten recursos adicionales que estan ya asginados a otros procesos, mientras esperan deben retener los recursos que tienen.

### **Modelacion de interbloqueos**
Los interbloqueos se pueden representar en un grafo dirigido. Tenemos 2 tipos de nodos(N): recursos y procesos, tambien se tienen arcos(A) que implican lo siguiente: si un proceso apunta al recurso, hablamos de un proceso que solicita un recurso y para un recurso que apunta un proceso, hablamos de un recurso asignado al proceso apuntado.
![[Pasted image 20260726182331.png]]

### **Métodos para manejar interbloqueos**  
Podemos dividir en 2 tipos de maneras de lidiar con los interbloqueos:  
#### **Ignorar el problema**  
La más simple de todas es el algoritmo avestruz o de Ostrich, qué consiste en ignorar el problema. Los S. O. que manejan este método parten de la hipótesis de que los usuarios preferirían bloqueos ocasionales y continuar con el uso los demás distintos recursos. Claro que se tienen problemas con éste método como en las restricciones de los procesos y en el uso de recursos.  
#### **Afrontar el problema**  
Se manejan 4 métodos donde 2 de ellos aseguran con un protocolo que nunca se entrará en un abrazo mortal y otro donde se permite que ocurra un abrazo mortal pero se recupere después, claro que recuperar es más difícil y caro.  

**Prevención de bloqueos**  
A grandes rasgos consiste en diseñar un sistema que excluya, a priorio, la posibilidad de interbloqueo. Se consigue asegurandonos de preveer qué ocurra una de las 4 condiciones de Coffman.  
- **Exclusión mutua**. Se tienes recursos que podrían ser manejados sin exclusión mutua, por ejemplo una impresora usando spooling, acá se hace uso de otro proceso que manejaria una cola de las impresiones, un proceso A y un proceso B pueden al mismo tiempo enviar a la cola de impresión y el spooler se encargará de imprimir ordenadamente, así preveemos el interbloqueo. Ocurre que hay recursos del sistema que no pueden permitirse este tipo de técnica, por ejemplo una tabla de procesos, transacciones, semáforos, qué deben tener exclusión mutua obligatoriamente y por lo cual debe buscar una prevención en las otras 3 condiciones.
- **Retener y esperar**. Para preveer un interbloqueos en la condicion de retener y esperar debemos asegurarnos de que el proceso que solicite algun recurso ya no tenga alguno asignado. El primer protocolo propone asignar al procesos todos los recursos necesarios antes de que su ejecucion, se podria lograr haciendo que las llamadas al sistema de asignacion de recursos suceda antes que las demas llamadas al sistema. El segundo protocolo propone que cada vez que el proceso no retenga algun recurso, recien es posible que solicite otro recurso. Para ambos protocolos surgen desventajas, como el poco uso de el conjunto de recursos necesarios, porque no seran utilizados la mayor parte del tiempo debido a que el ultimo recurso del conjunto se usara unicamente al final de todo pero estaria siendo ocupado todo ese tiempo. Una segunda desventaja aparece cuando un proceso trata de solicitar recursos muy solicitados, lo que podria generar inanicion.
- **No expropiacion**. Con el fin de asegurar que esta condicion no se cumpla, se propone el siguiente protocolo.- Aquel proceso que solicite recursos adicionales y esos recursos esten asignado a otro proceso, osea que debe esperar, entra a su lista de recursos necesarios, tanto los asignados como los solicitados. Ahora los procesos que soliciten otros recursos adicionales y veamos que pertenece a la lista de los recursos necesarios de otro proceso, teniendo obviamente como asignado el solicitado, es entonces que se los desasigna y asigna al solicitante. Un proceso puede ser reinicializado si consigue el recurso solicitado y alguno de los que tenia asignado. Dicho protocolo es usado para recursos del cual su estado es facilmente recuperable, por ejemplo los registros del CPU y el espacio en memoria. Por otro lado se tienen recursos como la impresora, que si fuera aplicado aqui, se tendrian resultados confusos.
- **Espera circular**. Se propone una solucion para preveer la espera circular, en la cual se asignaria un numero del conjunto de numero naturales a cada recurso de un conjunto de recursos y se haria una comparacion de que un recurso sea menor o mayor para poder ser asignado. Lo que deriva a la regla de que los procesos pueden solicitar recursos siempre y cuando devuelva el recurso que tenga y el que solicite sea mayor al que tiene. Basicamente estariamos generando un orden.

**Evitacion de interbloqueos**
Se plantea que independientemente del orden o metodo de solicitud de recursos por parte de los procesos, manejemos un control de las asignaciones y solicitudes por medio de algun tipo de restriccion, llevandolo a un estado seguro. La otra forma que se plantea es teniendo informacion adicional a la solicitud de recursos. Ambas buscan anticipar la posibilidad de interbloqueo y se aseguran de que no nunca exista esa posiblidad.
**Estado seguro e inseguro**
- Un estado **seguro** ocurre si existe posibilidad de interbloqueo 
