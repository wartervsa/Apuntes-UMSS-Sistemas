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
- **No expropiacion**. Los recursos ya asignados no pueden ser desasignados, debe ser el proceso el que lo haga voluntariamente o una vez terminada su tarea.
- **Espera circular**. Debe haber una cadena de 2 o mas procesos que entre todos esperen un recurso asignado al siguiente miembro de la cadena.
- **Retener y esperar**. Los procesos que contegan recursos pueden solicitar otros recursos adicionales. Debe existir un proceso que tenga al menos un recurso asignado y estar esperando por recursos adicionales que esten siendo retenidos por otros procesos.

### **Modelacion de interbloqueos**
Los interbloqueos se pueden representar en un grafo dirigido. Tenemos 2 tipos de nodos(N): recursos y procesos, tambien se tienen arcos(A) que implican lo siguiente: si un proceso apunta al recurso, hablamos de un proceso que solicita un recurso y para un recurso que apunta un proceso, hablamos de un recurso asignado al proceso apuntado.
![[Pasted image 20260726182331.png]]

### **Metodo para manejar el interbloqueo**
Podemos manejar 2 tipos de vision.- La que ignora el problema(algoritmo de avestruz o de Ostrich) y los que afrontan el problema. 


