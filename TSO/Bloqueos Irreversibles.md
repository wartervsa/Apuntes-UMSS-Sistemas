Es un sistema tenemos un numero de recursos limitados, estos recursos pueden ser:
- **Fisicos**.- Ciclos de CPU, espacio en memoria, dispositivos de E/S(impresora, teclado, mouse, disco duro, etc.)
- **Logicos**.- Archivos, tablas del sistema, registros de base de datos, semaforos, entre otros.
Estos son asignados a procesos que solicitan una cantidad de recursos, dicha solicitud no debe exceder los recursos existentes. Cada proceso debe solicitar esos recursos antes de usarlo y liberar al terminar de usar, tambien debe solicitar lo recursos necesarios para la llevar a cabo su tarea. Surgen momentos donde ciertos recursos no estan disponibles en el momento de la solicitud y deben esperar a que el proceso que los tiene asignados libere esos recursos, pero puede ocurrir que tambien este esperando recursos y asi sucesivamente, esto crea un bloqueo.
Los recursos son de 2 tipos:
- **Recursos Apropiativos**.- Son aquellos que pueden ser tomados del proceso que los posee sin producir un dano.
- **Recursos no Apropiativos**.- Son aquellos que no pueden ser tomados del proceso que los posee porque podria producir un fallo de calculo. En general los bloqueos se relacionan con recursos no apropiativos.

