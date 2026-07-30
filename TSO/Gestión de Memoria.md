Las componentes que se involucran en la gestión de la memoria son. - el gestor de memoria del sistema operativo, el lenguaje de programación, el compilador, el montador y el hardware de gestión de memoria. 
Cada proceso requiere de una imagen de memoria, donde una imagen de memoria clásica de divide en Texto, Datos y Pila, pero puede haber información adicional para dar soporte a los hilos o a las bibliotecas dinámicas. 
- **Región de Texto**. Contiene el código maquina(métodos, funciones, lógica matemática, operadores). 
- **Región de Datos**. Lleva las variables globales incializadas, las no incializadas y datos creados dinámicamente(heap), en este espacio se crean las variables dinámicas. 
- ****