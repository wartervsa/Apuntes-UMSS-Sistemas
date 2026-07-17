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
Para n procesos que contengan la misma sección crítica deben ser manejados con algun mecanismo que cumpla con ejecutar un solo proceso en la sección crítica y cumpla con funcionalidades básicas como:
- Cada proceso debe solicitar permiso con algún fragmento de código para entrar en la sección crítica. 
- Cuando un proceso termina de ejecutar la sección crítica debe comunicar su salida mediante otro fragmento de código. Su propósito es que aquellos procesos que solicitan permiso puedan entrar. 
Para cualquier tipo de mecanismo se debe cumplir con los siguientes 
