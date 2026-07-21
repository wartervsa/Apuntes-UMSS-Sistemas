Es un sistema tenemos un numero de recursos limitados, estos recursos pueden ser:
- **Fisicos**.- Ciclos de CPU, espacio en memoria, dispositivos de E/S(impresora, teclado, mouse, disco duro, etc.)
- **Logicos**.- Archivos, tablas del sistema, registros de base de datos, semaforos, entre otros.
Estos son asignados a procesos que solicitan una cantidad de recursos, dicha solicitud no debe exceder la cantidad de recursos existentes. Cada proceso debe solicitar esos recursos antes de usarlo y liberar al terminar de usar, un proceso debe solicitar lo recursos necesarios para la llevar a cabo su tarea. Surgen momentos donde ciertos recursos no estan disponibles en el momento de la solicitud y deben esperar a que el proceso que los tiene asignados libere esos recursos, pero puede ocurrir que tambien este esperando recursos y asi sucesivamente, esto genera un bloqueo.
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
espera un suceso que sólo puede originar otro proceso del mismo conjunto. Un ejemplo.-
![[Pasted image 20260721181242.png]]

