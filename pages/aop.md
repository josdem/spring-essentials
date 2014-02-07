# Programación orientada a aspectos

------

En software, varias actividades son comúnes a la mayoría de las aplicaciones. Logs, seguridad y la administración de transacciones son importantes, pero dichas acciones ¿deberían ser objetos involucrados dentro la aplicación en uso activo y constante?, o sería mejor si los objetos se enfocarán en los problemas de negocio y dejar ciertos aspectos a ser manejados por algiuien más.

En desarrollo de software, las funciones que se extienden a múltiples puntos de una aplicación son llamados _cross cutting concerns_. Típicamente, dichas preocupaciones transversales están conceptualmente separadas(pero a menudi embebebidos) de la lógica de la aplicación. La separación de las preocupaciones transversales de la lógica de negocio es donde la **programación orientada a aspectos** se ocupa bastante bien.

## Conceptos esenciales

Una _preocupación transversal(cross-cutting concern)_ es una funcionalidad que afecta múltiples puntos de una aplicación. Por ejemplo: seguridad, bitácorado, auditoría, medición de benchamrk, manejo de transacciones.

Una técnica común en la programación orientada objeto para reusar funcionalidad común es la herencia o la delegación. Pero la herencia puede conducir a una jerarquía de objetos frágiles si la misma clase base se utiliza en toda la aplicación, y la delegación puede ser incómoda porque se pueden requerir complicados llamadas al objeto delegado.

Con AOP, se puede definir la funcionalidad común en un lugar, pero se puede definir de forma declarativa como y donde debe ser aplicado, sin tener que modificar la clase en cuestión. Los beneficios, la lógica de negocion de cada _concern_ está ahora en un lugar, opuesto a ser arrastrado en todo el código base; adicionalmente, nuestros módulos de servicios son ahora más limpios por que sólo tienen nuestra preocupación principal que es la lógica de negocio, y las preocupaciones secundarias han sido movidas a aspectos.

<blockquote>
  <p>Mientras haces el diseño de tu aplicación no consideras el uso de AOP.</p>
</blockquote>

### Advice

Los aspectos tienen un propósito, un trabajo por hacer. En términos de AOP, el trabajo de un aspecto es llamado _advice_. Es decir, es código que se ejecutará en alguna parte de la aplicación. Los _advices_ definen el _qué_ y el _cuándo_ de un aspecto. Los aspectos de Spring pueden trabajar con 5 tipos de advice:

* Before
* After
* After-returning
* After-throwing
* Around

### JoinPoint

**Un _join point_ es un punto en la ejecución de la aplicación donde un aspecto puede ser conectado.** Este punto podría ser la llamada a ún método, cuando una excepción sea arrojada, o incluso si un campo es modificado. Esos son los puntos donde el código de un aspecto puede ser insertado dentro del flujo normal de la aplicación para agregar nuevo comportamiento.

### Pointcut

Los _pointcuts_ ayudan a reducir los _join points_ avisados por un aspecto. Es decir, son el predicado de un adivce. Si el advice define el _qué_ y el _cuándo_, los pointcuts definen el _dónde_. La definición de pointcut hace coincidir uno o más _join points_ a los cuales el _advice_ deberá estar tejido. A menudo se específican dichos pointcuts usando clases explícitas y nombres de métodos, o a través de expresiones regulares que definen los patrones de coincidencias en nombres de clases o métodos.

### Aspectos

Un aspecto es la combinación de advices y pointcuts. Tomados en conjunto, definen cada cosa que hay que saber acerca del aspecto en total, lo que hace, cuando lo hace y dónde lo hace.

#### Introductions

Aunque no son considerador en este entrenamiento, es bueno saber que un _introduction_ te permite agregar métodos o atributos a clases existentes. El nuevo método y variable de instancia pueden ser introducidos a clases existentes sin tener que cambiarlo, dándoles nuevo comportamiento y estado.

### Weaving

_Weaving_ es el proceso de aplicar aspectos a un _target object_ para crear un _proxy object_. Los aspectos son tejidos dentro del _target object_ en los _join points_ específicados. El weaving puedde tomar lugar en varios puntos en el ciclo de vida del _target object_:

* _Compile time_ - Los aspectos son tejidos cuando la clase es compilada. Esto requiere un compilador especial, en el caso de AspecjtJ lo hace de esta forma.
* _Classload time_ - Los aspectos son tejidos cuando la clase es cargada dentro de la JVM. Esto requiere un `ClassLoader` especial que mejora el código de byte de la clase antes de ser introducida a la aplicación.
* _Runtime_ - Los aspectos son tejidos en algún momento durante la ejecución de la aplicación. Típicamente, un contenedor de AOP generará dinámicamente un _proxy object_ que será delegado al _target object_ mientras se en los aspectos. Así es como Spring AOP funcionan.

<div class="bs-callout bs-callout-info">
<h4><i class="icon-coffee"></i> Información de utilidad</h4>
  <p>
    Te recomendamos que visites el sitio de <a href="http://eclipse.org/aspectj/">AspectJ</a> para más información.<br/>
    <b>En resumen:</b> Los join points son todos los puntos dentro del flujo de ejecución de la aplicación que son candidatos a tener aplicado un advice. El pointcut define donde el advice es aplicado. El concepto clave que debes tomar de esto es que los pointcuts definen cuales join points obtendran un advice.
  </a>
  </p>
</div>

## Declarando aspectos con Anotaciones

### Soporte de anotaciones con AspectJ

### Declaración de aspectos(Advice)

### Before

### After

### After returning

### After throwing

### Around

## JoinPoint en detalle

## Declarando aspectos con XML

