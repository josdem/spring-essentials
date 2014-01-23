# Elementos esenciales de Springframework

------

Spring es un framework Open Source, originalmente creado por Rod Johnson y descrito en su libro [_Expert One-on-one: J2EE Design and Development_](http://www.wrox.com/WileyCDA/WroxTitle/productCd-0764543857.html). Spring fue creado para direccionar la complejidad del desarrollo de una aplicación empresarial, y hace posible usar JavaBeans para mantener la simplicidad del desarrollo, así como, la creación de componentes complejos con conceptos muy simples como son Clases e Interfaces. Y aunque es mayormente usado en aplicaciones cliente-servidor, cualquier tipo de aplicación puede beneficiarse de Spring.

<blockquote>
<p>Spring simplifica de sobremanera el desarrollo Java.</p>
</blockquote>

Actualmente, Springframework es una plataforma Java que proveeo del soporte comprensivo de infraestructura para desarrollar aplicaciones para la JVM. Maneja y administra la infraestructura de tal manera que podemos enfocarnos en el desarrollo de la aplicación.

Al usar Spring tu puedes:

* Hacer que un método Java se ejecute en una transacción de base de datos sin tener que tratar con las API's de transacciones.
* Hacer que un método local en Java sea un procedimiento remoto sin tener que tratar con las API's de remote.
* Hacer un método local en Java una operación administrada sin tener que tratar con la API de JMX.
* Hacer un método local en Java un manejador de mensajes sin tener que tratar con la API de JMS.

Ahora bien, **¿cómo le hace Spring para eliminar toda esta complejidad?**. A grandes rasgos, se emplean 4 estrategias:

* El desarrollo debe ser ligero y minimamente invasivo con POJO's.
* Bajo acoplamiento a través de la inyección de dependencias y la orientación a interfaces.
* Programción declarativa a través de aspectos y convenciones comúnes.
* Reducción de código de plomería a través de aspectos y templates.

## ¿Qué es lo nuevo en Spring?

En Noviembre del 2007 el equipo de SpringSource libero la versión 2.5 del framework. El hecho más significativo de la versión fue la adopción generalizada del uso de anotaciones, pues previo a ello, todo era configuración XML. Sin embargo, hay otras cosas que destacar.

* Spring 2.5
    * Inyección de dependencias a través de la anotacion `@Autowired` y el control del ensamblaje con `@Qualifier`
    * Soporte para las anotaciones del JSR-250, incluyendo `@Resource` para la inyección de un recurso nombrado, así también `@PostConstruct` y `@PreDestroy` para el ciclo de vida.
    * Auto-detección de componentes de Spring que son anotados con `@Component`, el estereotipo principal de Spring
    * Todas las nuevas anotaciones para el modelo de programación de SpringMVC que simplifica enormemente el desarrollo web.
    * Una nueva integración con el framework de pruebas que esta basado en JUnit4 y el uso de anotaciones.
    * Soporte completo para Java 6 y JEE 5, incluyendo JDBC 4, JT 1.1, JavaMail 1.4 y JAX-WS 2.0
    * Una nueva expresión de Pointcut para tejer aspectos dentro de los Beans de Spring por su nombre.
    * Nuevos namespaces para configuración XML, incluyendo el de _context_ para configurar detalles del contexto de la aplicación y un namespace _jms_ para configurar message_driven beans.
    * Soporte para parámetros nombrados en `SqlJdbcTemplate`

Con la liberación de Spring 3.0(Diciembre del 2009), se adoptaron aún más el uso de anotaciones en varios aspectos del framework y un conjunto intenso de nuevas características:

* [Spring 3.0](http://spring.io/blog/2009/12/16/spring-framework-3-0-goes-ga)
    * Soporte a gran escala para REST en SpringMVC, incluyendo los controllers que responden a las URL's del estilo REST con XML, JSON, RSS o cualquier otra respuesta adecuada.
    * Un nuevo lenguaje de expresión que ofrece inyección de dependencias a un nuevo nivel habilitando la inyección de valores de una variedad de fuentes, incluyendo otros beans y propiedades de sistema.
    * Nuevas anotaciones para SpringMVC, incluyendo `@CookieValue` y `@Request-Header`, para jalar valores desde la información del navegador y el request.
    * Un nuevo namespace XML para una configuración más fácil de SpringMVC
    * Soporte para validaciones declarativas con anotaciones de JSR-303(Bean Validations).
    * Soporte para la especificación JSR-330 en inyección de dependencias.
    *y  Declaraciones basada en anotaciones de métodos calendarizados y asíncronos.
    * Una nueva configuración basada en anotaciones para librar a Spring de la configuración XML.
    * La funcionalidad de mapeo Objeto a XML del proyecto de Spring WebServices(OXM).
    * Ya no hay soporte para Java 1.4

El pasado Diciembre del 2013 se libero la versión 4 de Spring, con cambios y mejoras muy notables de mencionar, y aún mejor conocer, ya que se adaptan a las nuevas necesidades de las aplicaciones modernas:

* [Spring 4](http://spring.io/blog/2013/12/12/announcing-spring-framework-4-0-ga-release)
    * Esta versión es compatible con Java 1.6+.
    * Nuevas versiones de integración con frameworks como: Hibernate 3.6+, EhCache 2.1+, Quartz 1.8+, Groovy 1.8+, y Joda-Time 2.0+, Hibernate Validator 4.3+ y Jackson 2.0+.
    * Un POM llamado **Bill of Materials** para que los usuarios de Maven no conflictuen las versiones.
    * El soporte temprano para Java 8
    * Configuración del AppCtx con ayuda el lenguaje dinámico Groovy, demasiado interesante, pues provee de un DSL para hacerlo.
    * Soporte para Servlet 3.0.
    * Más soporte para REST a través de `@RestController` y el `AsyncRestTemplate`.
    * El nuevo soporte para WebSockets compatible con el JSR-356, en conjunto con [SockJS](https://github.com/sockjs/sockjs-client), a través de STOMP.
    * Mejor soporte de Testing con el uso de meta-anotaciones, el uso de profiles con `@ActiveProfiles`.
    * Y aunque no es parte de la liberación central, parte de esto motivo la liberación de [Spring Boot](http://projects.spring.io/spring-boot/docs/README.html).

## Módulos de Spring

Springframework esta formado en alrededor de 20 módulos. Dichos módulos están agrupados dentro del Core Container, la Integración y el Acceso a Datos, Web, AOP, la Instrumentación y Test.

![Alt spring-overview](img/spring-overview.png "Spring Overvivew")

### Escenarios de uso de Spring

#### Uso típico en una aplicación Web hecha totalmente con Spring

![Alt spring-overview](img/overview-full.png "Spring Overvivew")

------

#### Uso de Spring en la capa de servicios usando una librería web de terceros

![Alt spring-overview](img/overview-thirdparty-web.png "Spring Overvivew")

------

#### Uso de elementos remotos

![Alt spring-overview](img/overview-remoting.png "Spring Overvivew")

------

#### Intercambiando POJO's existentes

![Alt spring-overview](img/overview-ejb.png "Spring Overvivew")

------

## Diseño de aplicaciones(orientada a interfaces)

La mayoría de nosotros hemos tenido la mala experiencia de lidiar con una pieza de software que tiene un _mal diseño_. Inclusive muchos de nosotros hemos tratado de descubrir que quizo decir el autor con ese diseño. Pero ¿qué es un mal diseño?.

La mayoría de los desarrolladores no se propusieron crear _malos diseños_. Sin embargo, la mayoría del software se degrada finalmente al punto en que alguien dirá que el diseño es poco sólido. ¿Por qué ocurre esto? No fue el diseño pobre para empezar, o qué el diseño realmente esta en degradación como un pedazo de carne en mal estado. En el corazón de este problema es nuestra falta de una buena definición de _mal diseño_.

Hay un conjunto de criterios que creo que la mayoría de los desarrolladores consideran o están de acuerdo. Una pieza de software que cumpla con sus requisitos y sin embargo presente cualquiera o todos los siguientes tres rasgos tiene un mal diseño:

1. Es díficil de cambiar por que cada cambio afecta muchas otras partes del sistema.(Rígidez)
2. Cuando haces un cambio, partes inesperadas del sistema se rompen.(Fragilidad)
3. Es díficil de re-usar en otra aplicación por que no se puede desenredar de la aplicación actual.(Inmóvilidad)

Por otra parte, sería difícil demostrar que una pieza de software que no exhibe esos rasgos, es decir, que es flexible, robusto, y reutilizable, y que también cumple todos sus requisitos, tiene un mal diseño. Por lo tanto, podemos utilizar estos tres rasgos como una forma sin ambigüedades para decidir si un diseño es "bueno" o "malo".

### Creando aplicaciones con baja cohesión y alto acoplamiento

Si bien podemos crear aplicaciones con el entendimiento mínimo del requerimiento, y también siendo muy estrictos en su funcionamiento, encapsulando y colocando toda la funcionalidad en un sólo lugar puede causarnos muchos más problemas de los que potencialmente resolvería.

<div class="row">
  <div class="col-md-12">
    <h4><i class="icon-file"></i> TaskManager.java</h4>
    <script type="syntaxhighlighter" class="brush: java"><![CDATA[
package com.makingdevs.essentials;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.List;
import java.util.Vector;

public class TaskManager {

  public static void main(String[] args) throws IOException {
    Vector<String> tasks = new Vector<String>();
    System.out.println("Write a tasks list, ends with an empty task.");
    String task = "";
    do{
      BufferedReader in=new BufferedReader(new InputStreamReader(System.in));
      System.out.print("To Do: ");
      task = in.readLine();
      if(!task.equals(""))
        tasks.add(task);
    }while(!task.equals(""));
    System.out.println("You have " + tasks.size() + " tasks");
    System.out.println("Those are here: ");
    for(String t:tasks){
      System.out.println("* " + t);
    }
  }
}
    ]]></script>
  </div>
</div>

Ya tenemos una aplicación, ahora bien, que pasa si:

* Deseamos tener una entrada distinta a la línea del usuario, por ejemplo: Un String, una lista o un archivo.
* Queremos presentar la información en varios formatos y de diferentes formas.
* Deseamos modelar la clase encargada de almacenar la descripción de la tarea, e incluso determinar cuando se creó.
* Deseamos reutilizar este componente para crear tareas para varias equipos.
* Queremos saber quién creo la tarea y a quién se le va a asignar.
* La funcionalidad que deseemos agregar...

### Alta cohesión y bajo acoplamiento

Al tratar de crecer una aplicación, debemos de estar conscientes que la separación de componentes y responsabilidades es trivial para el éxito y funcionamiento

<div class="row">
  <div class="col-md-6">
    <h4><i class="icon-file"></i> TaskManagerIntegrationTests.java</h4>
    <script type="syntaxhighlighter" class="brush: java"><![CDATA[
package com.makingdevs.essentials;

import static org.junit.Assert.*;
import org.junit.Test;
import org.junit.Before;

import java.util.ArrayList;
import java.util.List;

public class TaskManagerIntegrationTests {

  private TaskManager taskManager;

  @Before
  public void setup(){
    taskManager = new TaskManager();
  }

  @Test
  public void aTaskManagerWithZeroTasks(){
    assertNotNull(taskManager);
    assertTrue(taskManager.howManyTasks() == 0);
  }

  @Test
  public void aTaskManagerWithOneTasks(){
    assertNotNull(taskManager);
    taskManager.addTask(new Task());
    assertTrue(taskManager.howManyTasks() == 1);
  }

  @Test
  public void addATaskFromAString(){
    assertNotNull(taskManager);
    taskManager.addTask("new task with String");
    assertTrue(taskManager.howManyTasks() == 1);
  }

  @Test
  public void addATasksFromAList(){
    assertNotNull(taskManager);
    List<Task> tasksToAdd = new ArrayList<Task>();
    tasksToAdd.add(new Task());
    tasksToAdd.add(new Task());
    taskManager.addTask(tasksToAdd);
    assertTrue(taskManager.howManyTasks() == 2);
  }

  @Test
  public void addATasksFromAFile(){
    // TODO: Implements the feature
    assertTrue(false);
  }

  @Test
  public void getATaskByIndex(){
    addATaskFromAString();
    Task task = taskManager.getTaskAt(1);
    assertNotNull(task);
  }

  @Test
  public void findTaskByDescription(){
    addATaskFromAString();
    Task task = taskManager.findTask("new task");
    assertNotNull(task);
  }

  @Test
  public void findAllTasksByDescription(){
    addATasksFromAList();
    List<Task> tasksFound = taskManager.findTasks("new task");
    assertTrue(tasksFound.size() == 2);
  }

}
    ]]></script>
  </div>
  <div class="col-md-6">
    <h4><i class="icon-file"></i> TaskManager.java</h4>
    <script type="syntaxhighlighter" class="brush: java"><![CDATA[
package com.makingdevs.essentials;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.List;
import java.util.Vector;

public class TaskManager {

  public int howManyTasks() {
    return 0;
  }

  public void addTask(Task task) {
  }

  public void addTask(String s) {
  }

  public void addTask(List<Task> tasksToAdd) {

  }

  public Task getTaskAt(int i) {
    return null;
  }

  public Task findTask(String s) {
    return null;
  }

  public List<Task> findTasks(String s) {
    return null;
  }
}
    ]]></script>
  </div>
</div>

En algunos casos y pensando en el paradigma escolar de Orientación a Objetos podemos concebir en soluciones basadas en las relaciones entre objetos que ya conocemos, tales como Herencia, Agregación y/o Composición. Lo cual nos permite separar y estructurar la forma en como los objetos se podrían comportar.

<div class="row">
  <div class="col-md-6">
    <h4><i class="icon-file"></i> TaskManager.java</h4>
    <script type="syntaxhighlighter" class="brush: java"><![CDATA[
public abstract class TaskManager extends TaskStore { // Siendo TaskStore y TaskManager abstractas

  public TaskManager(){
    super();
  }
  //....
}
    </script>
  </div>
  <div class="col-md-6">
    <h4><i class="icon-file"></i> TaskManager.java</h4>
    <script type="syntaxhighlighter" class="brush: java"><![CDATA[
public class TaskManager {

  private TaskStore taskStore;

  public TaskManager(){
    taskStore = new TaskStore();
  }
  //....
}
    </script>
  </div>
</div>

<div class="row">
  <div class="col-md-6">
    <h4><i class="icon-file"></i> TaskManagerScrum.java</h4>
    <script type="syntaxhighlighter" class="brush: java"><![CDATA[
public class TaskManagerScrum extends TaskManager {

  public TaskManagerScrum(){
    super();
  }
  //....
}
    </script>
  </div>
  <div class="col-md-6">
    <h4><i class="icon-file"></i> TaskManagerKanban.java</h4>
    <script type="syntaxhighlighter" class="brush: java"><![CDATA[
public class TaskManagerKaban extends TaskManager {

  public TaskManagerScrum(){
    super();
  }
  //....
}
    </script>
  </div>
</div>

Es aquí, en donde entra posibilidad de abstraer el comportamiento de los componentes que tienen responsabilidades por separados y pensar que inclusive se podrían y deberían de probar por separado, de tal manera que podríamos resusarlos o cambiarlos sin afectar a los demás.

Las interfaces surgen como el siguiente paso de la Programación Orientada a Objetos con la necesidad de agrupar y reutilizar las distintas funcionalidades de un objetode una forma más simple. Mediante interfaces podemos crear mejores diseños sin caer en las trampas de POO, así también, se crean nuevas y mejores formas de aplicar la implementación de un código de forma abstracta. Sin embargo debemos de tener presentes los 3 principios:

<blockquote>
  <p>
  <strong>
    La implementación de una interfaz debe hacer lo que sus métodos dicen que hacen.
  </strong>
  </p>
</blockquote>

<blockquote>
  <p>
  <strong>
    Una interfaz no debe interferir otros módulos de un programa o con otros programas.
  </strong>
  </p>
</blockquote>

<blockquote>
  <p>
  <strong>
    Si una implementación no es capaz de realizar su responsabilidad, debe notificar a quien lo llamó.
  </strong>
  </p>
</blockquote>

Cuando diseñamos de esta forma, podemos encontrar formas más elegantes de resolver un problema y centrar nuestra lógica de negocio en ello de tal manera que pueda llevarse a una prueba de unidad exclusiva de una funcionalidad.

<div class="row">
  <div class="col-md-6">
    <h4><i class="icon-file"></i> TaskManager.java</h4>
    <script type="syntaxhighlighter" class="brush: java"><![CDATA[
package com.makingdevs.essentials;

import java.util.List;

public class TaskManager {

  private TaskStore taskStore;

  public int howManyTasks() {
    return taskStore.count();
  }

  public void addTask(Task task) {
    taskStore.createTask(task);
  }

  public void addTask(String description) {
    Task task = new Task();
    // Nothing with description
    taskStore.createTask(task);
  }

  public void addTask(List<Task> tasksToAdd) {
    for(Task task:tasksToAdd){
      taskStore.createTask(task);
    }
  }

  public Task getTaskAt(int i) {
    return taskStore.readTask(new Integer(i).longValue());
  }

  public Task findTask(String description) {
    return taskStore.findTask(description);
  }

  public List<Task> findTasks(String description) {
    return taskStore.findAllTasks(description);
  }
}
    </script>
  </div>
  <div class="col-md-6">
    <h4><i class="icon-file"></i> TaskStore.java</h4>
    <script type="syntaxhighlighter" class="brush: java"><![CDATA[
package com.makingdevs.essentials;

import java.util.List;

public interface TaskStore {
  void createTask(Task task);
  Task readTask(Long pk);
  Task findTask(String description);
  List<Task> findAllTasks(String description);
  int count();
}
    </script>
  </div>
</div>

Siempre se han confundido las pruebas de integración con las pruebas de unidad. Nuestra concepción debe ampliarse más para entender los preceptos de la terminología. Para  explicar esto detallemos lo siguiente de manera informal:

* Pruebas de unidad: Son del tipo que determinan si un componente funciona de cierta forma esperada, bajos ciertos escenarios controlados y bien definidos, y excluyen el buen o mal funcionamiento de los elementos que colaboran con el mismo, pues no es de la incumbencia del elemento bajo ejecución, pero deben indicarnos que los mensajes a dichos elementos ajenos y externos deben ser enviados.
* Pruebas de integración: Son aquellas que determinan que un componente se ejecuta correctamente incluyendo las interacciones con sus componentes asociados, los cuales también deben estar listos para ser invocados.
* Pruebas de sistema: Son aquellas que ayudan a determinar si un flujo de negocio se ejecuto de manera correcta dadas ciertas condiciones y ambientes, y por lo general incluyen a varios componentes en su estructura.

#### Mockito

[Mockito](https://code.google.com/p/mockito/) es un framework de mocking. Permite escribir pruebas con una API simple y limpia. Mockito es poderoso debido a que las pruebas son muy fácil de leer y que producen errores de verificación de limpieza con orden.

Mockito casi no tiene API's. en verdad se necesita de muy poco para crear mocks, pues solo hay un tipo de mock y hay solo una forma de crearlos. Sólo hay que recordar una cosa muy simple: **El stub(simulación) va antes de la ejecución, las verificaciones de las interacciones van después.**

Algunas otras características son:

* Se pueden crear mocks en clases concretas o interfaces.
* Se usa la anotación `@Mock` para los colaboradores.
* La verificación de errores es limpia.
* Permite una verificaicón flexible en orden.
* Soporte verificaciones de un número exacto de invocaciones o de por lo menos alguna.
* Algunas otras más.

<div class="row">
  <div class="col-md-12">
    <h4><i class="icon-file"></i> TaskManagerUnitTests.java</h4>
    <script type="syntaxhighlighter" class="brush: java"><![CDATA[
package com.makingdevs.essentials;

import org.junit.Test;
import org.junit.runner.RunWith;
import org.mockito.InjectMocks;
import org.mockito.Mock;
import org.mockito.Mockito;
import org.mockito.invocation.InvocationOnMock;
import org.mockito.runners.MockitoJUnitRunner;
import org.mockito.stubbing.Answer;

import java.util.ArrayList;
import java.util.List;

import static org.junit.Assert.assertNotNull;
import static org.junit.Assert.assertTrue;
import static org.mockito.Matchers.eq;
import static org.mockito.Mockito.*;

@RunWith(MockitoJUnitRunner.class)
public class TaskManagerUnitTests {

  @Mock
  TaskStore taskStore;

  @InjectMocks
  TaskManager taskManager = new TaskManager();

  @Test
  public void aTaskManagerWithZeroTasks(){
    when(taskStore.count()).thenReturn(0);
    assertTrue(taskManager.howManyTasks() == 0);
    verify(taskStore).count();
  }

  @Test
  public void aTaskManagerWithOneTasks(){
    Task taskMock = new Task();
    when(taskStore.count()).thenReturn(1);
    taskManager.addTask(taskMock);
    assertTrue(taskManager.howManyTasks() == 1);
    verify(taskStore).createTask(taskMock);
    verify(taskStore).count();
  }

  @Test
  public void addATaskFromAString(){
    Task taskMock = new Task();
    doNothing().when(taskStore).createTask(any(Task.class));
    when(taskStore.count()).thenReturn(1);
    taskManager.addTask("new task with String");
    assertTrue(taskManager.howManyTasks() == 1);
    verify(taskStore).createTask(any(Task.class));
    verify(taskStore).count();
  }

  @Test
  public void addATasksFromAList(){
    assertNotNull(taskManager);
    List<Task> tasksToAdd = new ArrayList<Task>();
    tasksToAdd.add(new Task());
    tasksToAdd.add(new Task());
    when(taskStore.count()).thenReturn(2);
    doNothing().when(taskStore).createTask(any(Task.class));
    taskManager.addTask(tasksToAdd);
    assertTrue(taskManager.howManyTasks() == 2);
    verify(taskStore,times(2)).createTask(any(Task.class));
    verify(taskStore).count();
    // atLeastOnce()
    // atLeast(2)
    // atMost(5)
  }

  @Test
  public void addATasksFromAFile(){
    // TODO: Implements the feature
    assertTrue(false);
  }

  @Test
  public void getATaskByIndex(){
    addATaskFromAString();
    when(taskStore.readTask(1L)).thenReturn(new Task());
    Task task = taskManager.getTaskAt(1);
    assertNotNull(task);
    verify(taskStore).readTask(1L);
  }

  @Test
  public void findTaskByDescription(){
    addATaskFromAString();
    when(taskStore.findTask("new task")).thenReturn(new Task());
    Task task = taskManager.findTask("new task");
    assertNotNull(task);
    verify(taskStore).findTask("new task");
  }

  @Test
  public void findAllTasksByDescription(){
    addATasksFromAList();
    List<Task> tasksMocked = new ArrayList<Task>();
    tasksMocked.add(new Task());
    tasksMocked.add(new Task());
    when(taskStore.findAllTasks("new task")).thenReturn(tasksMocked);
    List<Task> tasksFound = taskManager.findTasks("new task");
    assertTrue(tasksFound.size() == 2);
    verify(taskStore).findAllTasks("new task");
  }

}
    </script>
  </div>
</div>

<div class="bs-callout bs-callout-info">
<h4><i class="icon-coffee"></i> Información de utilidad</h4>
  <p>
    Te recomendamos que explores <a href="http://docs.mockito.googlecode.com/hg/latest/org/mockito/Mockito.html">la documentación de Mockito</a>, que aunque es simple, cubre la mayoría de los casos que se puedan presentar en cualquier aplicación empresarial, y te ayudarán a comprobar el comportamiento de tu aplicación.
  </a>
  </p>
</div>