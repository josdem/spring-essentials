# Uso de SpEL y Profiles

------

## Spring Expression Language

El lenguaje de Expresión de Spring es un poderoso lenguaje de expresión ue soporta la búsqueda y manipulación de un objeto como grafo en tiempo de ejecución. El cual, fue creado para ser usado en varios subproyectos de Spring.

No esta directamente atado a Spring, por lo que puede ser usado de forma independiente(es decir, no necesita un `ApplicationContext`). Aunque mayormente, estremos usando el SpEL dentro del contenedor de Spring.

Le lenguaje de expresión soporta la siguiente funcionalidad:

* Expresion de literales
* Operadores relacionales y booleanos
* Expresiones regulares
* Expresiones de clases
* Acceder a propiedades, arreglos, listas y mapas
* Invocación de métodos
* Asignaciones
* Llamadas a constructores
* Referencias a beans
* Construcción de arreglos
* Listas en línea
* Operador ternario
* Variables
* Funciones definidas por el usuario
* Proyección de colecciones
* Selección de colecciones
* Expresiones en plantillas

La interface `ExpressionParser` es responsable del parseo de las expresiones, puede arrojar dos excepciones: `ParseException` y `EvaluationException`; y los paquetes de uso principales para el manejo de SpEL son: `org.springframework.expression` y el subpaquete `spel.support`.

## Spring Profiles

