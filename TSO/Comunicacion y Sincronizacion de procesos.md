La concurrencia en terminos generales: dicho de diferentes personas, sucesos o cosas, que se juntan en el mismo lugar y tiempo. La concurrencia en los sistemas informaticos ocurre tanto a nivel de hardware como software.
![[Pasted image 20260716093015.png]]
La complejidad de los procesadores actuales trae consigo tecnicas de pipe-line y superescalares que permiten ejecutar varias operaciones de forma simultanea, tambien tecnicas de multihilo(Hyper o multithreading) que permiten ejecutar de forma alternada 2 mas flujos de trabajo(hilos).
La concurrencia entre procesos se puede dividir en independientes y cooperantes. Los procesos independientes funcionan sin requerir de cooperacion o ayuda de otros procesos,
los procesos cooperantes estan diseñados para trabajar conjuntamente en alguna actividad, requiriendo una comunicacion y sincronizacion entre ellos.
Para un proceso independiente o cooperante se pueden tener 2 tipos de interacciones:
- Interaccion motivada entre procesos que comparten o compiten por recursos fisicos o lógicos.
- Interaccion motivada entre procesos que se comunican y sincronizan para alcanzar un objetivo común.
Este tipo de interacciones obligan al sistema operativo a emplear mecanismos y servicios que logren una correcta comunicación y sincronización entre procesos(IPC InterProcess Communication).
## **Programacion concurrente**
Muchos problemas se pueden resolver mas fácilmente o eficientemente con la cooperacion entre procesos(o hebras), la tecnica que aplica esto se llama programacion concurrente, que trae consigo unos problemas que no ocurren en la programacion secuencial.
**Seccion critica**
Es un momento en que las intrucciones del programa de 2 o mas procesos coinciden con una variable común, o en general, con algun recurso. Por ejemplo en una suma donde ambos procesos actualizan una variable global, surge el problema, si no hay un control de la concurrencia entre los flujos de trabajo(hilos) porque compiten al querer actualizar la misma variable al mismo tiempo. Esta competencia por datos es conocida como *race condition* que es cuando el resultado depende del orden particular en que se intercalan las operaciones de procesos concurrentes. Su solución es asegurar, de alguna manera, que una sola hebra(hilo) actualice la variable compartida. 
Para n procesos que contengan la misma sección crítica deben ser manejados con algun tipo de mecanismo que cumpla con ejecutar un solo proceso en la sección crítica y cumpla con funcionalidades básicas como:
- Cada proceso debe solicitar permiso con algún fragmento de código para entrar en la sección crítica. 
- Cuando un proceso termina de ejecutar la sección crítica debe comunicar su salida mediante otro fragmento de código. Permitiendo que otros procesos puedan ingresar a la sección crítica. 
Cualquier solución debe cumplir con estos 3 requisitos:
- **Exclusión Mutua**. - Un solo proceso puede estar ejecutando la sección crítica. 
- **Progreso**. - En un tiempo finito se debe delegar la ejecución de la sección crítica únicamente a los procesos que desean entrar. 
- **Espera acotada(entrada garantizada - ausencia de inanición)**. - Debe haber un límite de veces para los procesos que ejecuten una sección crítica después de que haya otro proceso que solicite la entrada. 
Se tienen varios intentos de solución ante la concurrencia, como por ejemplo. - 
**Inhabilitación de interrupciones**. Que consistia en inhibir(impedir/reprimir) todas las interrupciones una vez entra a la región crítica. Peligroso porque están dando autoridad a un proceso usuario y podría colgarse el sistema. 
**Variables de Bloqueo**. Se tiene una variable que maneja 2 estados: 0 y 1, 0 significa que no hay procesos en la sección crítica y 1 que si hay. El problema surge cuando más de un proceso, por el planificador, entran justamente a cambiar este valor, es decir la CPU es cedida justamente cuando esta apunto de cambiar el valor y por ende no detecta que ya está entrando 1 proceso y tendríamos 2 en la sección crítica. 
**Solución con ayuda del hardware**. - Muchos sistema implementan Instrucciones especiales para resolver los problemas de la sección crítica. Consiste en llevar a cabo, de alguna manera, que 2 o 3 operaciones se ejecuten indivisiblemente o atómicamente, es decir que se ejecutan esa instrucción hasta terminar evitando que otros interfieran.
### **Semáforo y Monitores**
He aquí las soluciones más eficaces, qué cumplen con los requisitos. 
#### **Semáforo**
El inconveniente con la solucion anterior es que va constantemente consultando si se puede o no acceder derrochando CPU. Ahora una solución más versátil para problemas más complejos o distintos de concurrencia son los semáforos, qué aportan un visión panorámica al sistema operativo. Los semáforos son un tipo abstracto de dato que puede manipularse mediante 2 operaciones: **P**(wait o down) y **V**(signal o up). 
![[Pasted image 20260720222723.png]]
La operación **P**(wait) simplemente comprueba si el valor es mayor a 0, en caso de que cumpla, decrementa el valor(del semáforo o señal se podría decir), ahora en el caso de no cumplir igual se decrementa, pero al ser menor que 0 se bloquea al proceso. Una forma de verlo es que se maneja al semáforo como la cantidad de recursos disponibles, una vez no tenga recursos solamente espero. Esto ocurre como operación atómica, todo o nada. Esa atomicidad es la única forma de resolver estos problemas de sincronización y evitar que se produzcan condiciones de competencia. 
La operación **V**(signal) se encarga de incrementar el valor del semáforo, seguido de la pregunta de si el valor es menor o igual a 0, qué en caso de cumplir sigue sin más, para el caso de no cumplir manda un señal al semáforo el cual se incrementó el valor y despierta a cualquier proceso dormido por ese semáforo, puede ser de forma aleatoria. Acá aplicando esa forma de verlo, podemos ver como la liberación del recurso y de dar turno a los que estaban esperando, si es que hubieran claro. De igual manera es una operación indivisible/atómica. La operación signal nunca bloquea al proceso que la ejecuta. 
#### **Monitores**
 Son la forma "mejorada" de los semáforos, ya que a pesar de solucionar el problema con los semáforos igual pueden haber problemas de omisión o mala ubicación de un wait o signal. 
Los monitores son una herramienta de sincronización más estructurada qué encapsula variables compartidas en conjunto con los procedimientos para accesar a esa variables. 
![[Pasted image 20260720222650.png]]
El monitor es el encargado de realizar la exclusión mutua, un solo proceso puede entrar en el monitor. Los monitores son construcciones del lenguaje, por lo cual el compilador sabe y maneja de forma especial. Lo primero que hace es asegurarse que no haya algún proceso en el monitor, en caso de cumplir entra directamente y en caso de no cumplir se suspende hasta que se libere el monitor. Al ser el compilador y no el programador el que se encarga de la exclusión mutua resulta más complicado que surjan errores, es suficiente con colocar la sección crítica en los procedimientos del monitor. 
En consecuencia, un monitor es un tipo de dato abstracto, y por lo tanto:
- Todos pueden acceder a los procedimientos del monitor, pero no a las variables encapsuladas. 
- Los procedimientos del monitor **no** pueden acceder a variables externas al monitor, es decir: solo accede a variables permanentes en el monitor, variables locales del procedimiento y sus parámetros. 
- Las variables del monitor debes estar incializadas antes que ningún procedimiento sea ejecutado. 
Para cumplir con esa sincronización se manejan **variables de condición**, éstas variables de condición 'c' asocian a una cola de procesos. 
- **waitC(c)**. Bloquea al proceso invocador y libera el monitor. 
- **signalC(c)**. - Desbloquea al proceso que esta al frente de la cola asociada a 'c', si es que existe. Podemos tener 2 maneras de proceder. - **signal-and-wait** donde el proceso que da la señal continua en el monitor y el proceso desbloqueado espera para ingresar nuevamente al monitor y también tenemos **signal-and-wait** donde el proceso señalizador suspende su ejecución en favor de proceso desbloqueado, el señalizador debe esperar para poder regresar al monitor y posiblemente compitiendo con otros procesos.
Dato relevante: La señal waitC(c) no es un contador como en los semáforos qué es necesario para su uso posterior. En el caso de monitores si no se encuentra proceso/s en la cola de procesos, simplemente se pierde la esa señal. 