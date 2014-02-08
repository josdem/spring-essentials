# Programación orientada a aspectos

------

En software, varias actividades son comúnes a la mayoría de las aplicaciones. Logs, seguridad y la administración de transacciones son importantes, pero dichas acciones ¿deberían ser objetos involucrados dentro la aplicación en uso activo y constante?, o sería mejor si los objetos se enfocarán en los problemas de negocio y dejar ciertos aspectos a ser manejados por algiuien más.

En desarrollo de software, las funciones que se extienden a múltiples puntos de una aplicación son llamados _cross cutting concerns_. Típicamente, dichas preocupaciones transversales están conceptualmente separadas(pero a menudi embebebidos) de la lógica de la aplicación. La separación de las preocupaciones transversales de la lógica de negocio es donde la **programación orientada a aspectos** se ocupa bastante bien.

<blockquote>
  <p>
    El AOP es usado en Spring para...<br>
    <ul>
      <li>... proveer servicios empresariales declarativos, especialmente como reemplazo de los servicios EJB's. El servicio más importante es la administración declarativa de transacciones</li>
      <li>... permitir a los usuarios implementar aspectos personalizados, complementando el uso de POO con AOP.</li>
    </ul>
  </p>
</blockquote>

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

### Target object

Es el objeto siendo avisado por varios por uno o más aspectos. Además conocido como el _advice object_. Este objeto por lo general será un objeto proxy.

### AOP Proxy

Es un objeto creado por el framework de AOP en orden para implementar los contratos del aspecto. En Spring, un proxy AOP será un proxy dinámico del JDK o un proxy CGLIB.

### Weaving

_Weaving_ es el proceso de aplicar aspectos a un _target object_ para crear un _proxy object_. Los aspectos son tejidos dentro del _target object_ en los _join points_ específicados. El weaving puedde tomar lugar en varios puntos en el ciclo de vida del _target object_:

* _Compile time_ - Los aspectos son tejidos cuando la clase es compilada. Esto requiere un compilador especial, en el caso de AspecjtJ lo hace de esta forma.
* _Classload time_ - Los aspectos son tejidos cuando la clase es cargada dentro de la JVM. Esto requiere un `ClassLoader` especial que mejora el código de byte de la clase antes de ser introducida a la aplicación.
* _Runtime_ - Los aspectos son tejidos en algún momento durante la ejecución de la aplicación. Típicamente, un contenedor de AOP generará dinámicamente un _proxy object_ que será delegado al _target object_ mientras se en los aspectos. Así es como Spring AOP funcionan.

<blockquote>
  <p>Los advices de Spring son escritos en Java.</p>
</blockquote>

<div class="bs-callout bs-callout-info">
<h4><i class="icon-coffee"></i> Información de utilidad</h4>
  <p>
    Te recomendamos que visites el sitio de <a href="http://eclipse.org/aspectj/">AspectJ</a> para más información.<br/>
    <b>En resumen:</b> Los join points son todos los puntos dentro del flujo de ejecución de la aplicación que son candidatos a tener aplicado un advice. El pointcut define donde el advice es aplicado. El concepto clave que debes tomar de esto es que los pointcuts definen cuales join points obtendran un advice.
  </a>
  </p>
</div>

El soporte de Spring para AOP viene en 4 formas:

* Spring AOP clásico basado en objetos proxy
* Aspectos manejados por anotaciones con `@AspectJ`
* Aspectos POJO puros
* Aspectos inyectados de AspectJ(disponibles en todas las versiones de Spring)

<div class="bs-callout bs-callout-warning">
<h4><i class="icon-coffee"></i> Información de utilidad</h4>
  <p>
  Y aunque el soporte clásico de Spring es base para aquellos manejados por aspectos y los de POJO puros, no los trataremos a profundidad, sin embargo, es bueno revisar y entender <a hrfe="http://docs.spring.io/spring/docs/4.0.1.RELEASE/spring-framework-reference/html/aop.html#aop-understanding-aop-proxies">los proxies de AOP en la documentación de Spring</a>.
  </p>
</div>

En Spring, los aspectos son tejidos dentro de los beans administrados por Spring en tiempo de ejecución cambiándolos por una clase proxy. Entre el tiempo cuando el proxy intercepta la llamada al método y el tiempo cuando invoca el método del target bean, el proxy ejecuta la lógica del aspecto. Spring no crea objetos proxy hasta que el bean proxy es necesario para la aplicación. Es por esto último que no necesitamos un compilador de AOP, ya que todo sucede en runtime.

<blockquote>
  <p>Spring sólo soporta Join Points de métodos.</p>
</blockquote>

<div class="row">
  <div class="col-md-6">
    <h4><i class="icon-file"></i> UserServiceLoggedImpl.java</h4>
    <script type="syntaxhighlighter" class="brush: java"><![CDATA[
package com.makingdevs.practica17;

import org.apache.commons.logging.Log;
import org.apache.commons.logging.LogFactory;
import org.springframework.stereotype.Service;

import com.makingdevs.model.User;
import com.makingdevs.services.UserService;

@Service
public class UserServiceLoggedImpl implements UserService {

  private Log log = LogFactory.getLog(UserServiceLoggedImpl.class);

  @Override
  public User findUserByUsername(String username) {
    log.debug("findUserByUsername : params = [" + username + "]");
    return null;
  }

  @Override
  public User createUser(String username) {
    log.debug("createUser : params = [" + username + "]");
    return null;
  }

  @Override
  public void addToProject(String username, String codeName) {
    log.debug("addToProject : params = [" + username + "," + codeName + "]");
  }

}
    ]]></script>
  </div>
  <div class="col-md-6">
    <h4><i class="icon-file"></i> TaskServiceLoggedImpl.java</h4>
    <script type="syntaxhighlighter" class="brush: java"><![CDATA[
package com.makingdevs.practica17;

import org.apache.commons.logging.Log;
import org.apache.commons.logging.LogFactory;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import com.makingdevs.model.Task;
import com.makingdevs.model.TaskStatus;
import com.makingdevs.services.TaskService;
import com.makingdevs.services.UserService;

@Service
public class TaskServiceLoggedImpl implements TaskService {
  
  private Log log = LogFactory.getLog(TaskServiceLoggedImpl.class);
  
  @Autowired
  UserService userService;

  @Override
  public Task createTaskForUserStory(String taskDescription, Long userStoryId) {
    // TODO Auto-generated method stub
    return null;
  }

  @Override
  public void assignTaskToUser(Long taskId, String username) {
    log.debug("Starting: assignTaskToUser");
    userService.findUserByUsername(username);
    log.debug("Ending: assignTaskToUser");
  }

  @Override
  public void changeTaskStatus(Long taskId, TaskStatus taskStatus) {
    // TODO Auto-generated method stub
    
  }

}
    ]]></script>
  </div>
</div>

<div class="row">
  <div class="col-md-12">
    <h4><i class="icon-file"></i> LoggingServicesTests.java</h4>
    <script type="syntaxhighlighter" class="brush: java"><![CDATA[
package com.makingdevs.practica17;

import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;
import org.springframework.util.Assert;

import com.makingdevs.services.TaskService;
import com.makingdevs.services.UserService;

@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(locations={"LoggingAppCtx.xml"})
public class LoggingServicesTests {
  
  @Autowired
  UserService userService;
  @Autowired
  TaskService taskService;

  @Test
  public void testUserService() {
    Assert.notNull(userService);
    userService.createUser("EmilyThorn");
  }
  
  @Test
  public void testTaskService() {
    Assert.notNull(taskService);
    taskService.assignTaskToUser(1L, "MakingDevs");
  }

}
    ]]></script>
  </div>
</div>

## Declarando aspectos

### Definiendo advices

<div class="row">
  <div class="col-md-4">
    <h4><i class="icon-file"></i> AfterAdvice.java</h4>
    <script type="syntaxhighlighter" class="brush: java"><![CDATA[
package com.makingdevs.practica18;

import org.apache.commons.logging.Log;
import org.apache.commons.logging.LogFactory;
import org.aspectj.lang.annotation.After;
import org.aspectj.lang.annotation.Aspect;
import org.springframework.stereotype.Component;

@Aspect
@Component
// This is the trick baby!!!
public class AfterAdvice {

  /**
   * You may register aspect classes as regular beans in your Spring XML
   * configuration, or autodetect them through classpath scanning - just like
   * any other Spring-managed bean. However, note that the @Aspect annotation is
   * not sufficient for autodetection in the classpath
   */

  private Log log = LogFactory.getLog(AfterAdvice.class);

  @After("execution(public * *(..))")
  public void afterMethod() {
    log.debug("After method advice");
  }

}
    ]]></script>
  </div>
  <div class="col-md-4">
    <h4><i class="icon-file"></i> BeforeAdvice.java</h4>
    <script type="syntaxhighlighter" class="brush: java"><![CDATA[
package com.makingdevs.practica18;

import org.apache.commons.logging.Log;
import org.apache.commons.logging.LogFactory;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Before;
import org.springframework.stereotype.Component;

@Aspect
@Component
public class BeforeAdvice {
  
  private Log log = LogFactory.getLog(BeforeAdvice.class);
  
  @Before("execution(public * *(..))")
  public void beforeMethod() {
    log.debug("Before method advice");
  }

}
    ]]></script>
  </div>
  <div class="col-md-4">
    <h4><i class="icon-file"></i> AfterReturningAdvice.java</h4>
    <script type="syntaxhighlighter" class="brush: java"><![CDATA[
package com.makingdevs.practica18;

import org.apache.commons.logging.Log;
import org.apache.commons.logging.LogFactory;
import org.aspectj.lang.annotation.AfterReturning;
import org.aspectj.lang.annotation.Aspect;
import org.springframework.stereotype.Component;

@Aspect
@Component
public class AfterReturningAdvice {

  private Log log = LogFactory.getLog(AfterReturningAdvice.class);

  @AfterReturning("execution(public * *(..))")
  public void afterReturningMethod() {
    log.debug("After returning method advice");
  }

}
    ]]></script>
  </div>
</div>

<div class="row">
  <div class="col-md-6">
    <h4><i class="icon-file"></i> TaskServiceEmptyImpl.java</h4>
    <script type="syntaxhighlighter" class="brush: java"><![CDATA[
package com.makingdevs.practica18;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import com.makingdevs.model.Task;
import com.makingdevs.model.TaskStatus;
import com.makingdevs.services.TaskService;
import com.makingdevs.services.UserService;

@Service
public class TaskServiceEmptyImpl implements TaskService {
  
  @Autowired
  UserService userService;

  @Override
  public Task createTaskForUserStory(String taskDescription, Long userStoryId) {
    // TODO Auto-generated method stub
    return null;
  }

  @Override
  public void assignTaskToUser(Long taskId, String username) {
    userService.findUserByUsername(username);
  }

  @Override
  public void changeTaskStatus(Long taskId, TaskStatus taskStatus) {
    throw new RuntimeException("WTF fail!!!!");
  }

}
    ]]></script>
  </div>
  <div class="col-md-6">
    <h4><i class="icon-file"></i> UserServiceEmptyImpl.java</h4>
    <script type="syntaxhighlighter" class="brush: java"><![CDATA[
package com.makingdevs.practica18;

import org.springframework.stereotype.Service;

import com.makingdevs.model.User;
import com.makingdevs.services.UserService;

@Service
public class UserServiceEmptyImpl implements UserService {

  @Override
  public User findUserByUsername(String username) {
    return null;
  }

  @Override
  public User createUser(String username) {
    return null;
  }

  @Override
  public void addToProject(String username, String codeName) {
    throw new RuntimeException("Cannot find project or username");
  }

}
    ]]></script>
  </div>
</div>

<div class="row">
  <div class="col-md-6">
    <h4><i class="icon-file"></i> AdvicesAppCtx.xml</h4>
    <script type="syntaxhighlighter" class="brush: xml"><![CDATA[
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xmlns:context="http://www.springframework.org/schema/context"
  xmlns:aop="http://www.springframework.org/schema/aop"
  xsi:schemaLocation="http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-4.0.xsd
    http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
    http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.0.xsd">

  <!-- HEY! this is fundamental, keep one eye here! ... -->   
  <aop:aspectj-autoproxy/>
  
  <context:component-scan base-package="com.makingdevs.practica18"/>

</beans>
    ]]></script>
  </div>
  <div class="col-md-6">
    <h4><i class="icon-file"></i> AdvicedServicesTests.java</h4>
    <script type="syntaxhighlighter" class="brush: java"><![CDATA[
package com.makingdevs.practica18;

import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;
import org.springframework.util.Assert;

import com.makingdevs.services.TaskService;
import com.makingdevs.services.UserService;

@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(locations={"AdvicesAppCtx.xml"})
public class AdvicedServicesTests {
  
  @Autowired
  UserService userService;
  @Autowired
  TaskService taskService;

  @Test
  public void testUserService() {
    Assert.notNull(userService);
    userService.createUser("EmilyThorn");
  }
  
  @Test
  public void testTaskService() {
    Assert.notNull(taskService);
    taskService.assignTaskToUser(1L, "MakingDevs");
  }
  
  @Test(expected=RuntimeException.class)
  public void testWithException() {
    userService.addToProject("makingdevs", "spring-essentials");
  }

}
    ]]></script>
  </div>
</div>

------

<div class="row">
  <div class="col-md-4">
    <h4><i class="icon-file"></i> AfterAdvice.java</h4>
    <script type="syntaxhighlighter" class="brush: java"><![CDATA[
package com.makingdevs.practica19;

import org.apache.commons.logging.Log;
import org.apache.commons.logging.LogFactory;
import org.aspectj.lang.JoinPoint;
import org.aspectj.lang.annotation.After;
import org.aspectj.lang.annotation.Aspect;
import org.springframework.stereotype.Component;

@Aspect
@Component
public class AfterAdvice {

  private Log log = LogFactory.getLog(AfterAdvice.class);

  @After("execution(* createUser*(..))")
  public void afterMethod(JoinPoint joinPoint) {
    log.debug("After advice method in " + joinPoint.getSignature().getName() + " with arguments:");
    for(Object o:joinPoint.getArgs()){
      log.debug("\t - " + o);
    }
  }
}
    ]]></script>
  </div>
  <div class="col-md-4">
    <h4><i class="icon-file"></i> AfterThrowingAdvice.java</h4>
    <script type="syntaxhighlighter" class="brush: java"><![CDATA[
package com.makingdevs.practica19;

import org.apache.commons.logging.Log;
import org.apache.commons.logging.LogFactory;
import org.aspectj.lang.JoinPoint;
import org.aspectj.lang.annotation.AfterThrowing;
import org.aspectj.lang.annotation.Aspect;
import org.springframework.stereotype.Component;

@Aspect
@Component
public class AfterThrowingAdvice {

  private Log log = LogFactory.getLog(AfterThrowingAdvice.class);

  // Look ma! I'm catching exceptions with AOP !!!
  @AfterThrowing(pointcut="execution(public * *(..))",throwing="customNameException")
  public void afterReturningMethod(JoinPoint joinPoint, RuntimeException customNameException) {
    StringBuffer buffer = new StringBuffer("Ha ocurrido un error en " + joinPoint.getSignature().getName() + " ");
    buffer.append("de " + joinPoint.getTarget().getClass().getName() + " - Argumentos:");
    for(Object o:joinPoint.getArgs()){
      buffer.append(o + " ");
    }
    buffer.append(" y el error " + customNameException.getMessage());
    log.error(buffer.toString());
  }

}
    ]]></script>
  </div>
  <div class="col-md-4">
    <h4><i class="icon-file"></i> BeforeAdvice.java</h4>
    <script type="syntaxhighlighter" class="brush: java"><![CDATA[
package com.makingdevs.practica19;

import org.apache.commons.logging.Log;
import org.apache.commons.logging.LogFactory;
import org.aspectj.lang.JoinPoint;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Before;
import org.springframework.stereotype.Component;

@Aspect
@Component
public class BeforeAdvice {
  
  private Log log = LogFactory.getLog(BeforeAdvice.class);
  
  @Before("execution(* com.makingdevs.practica18.*Service*.*(..))")
  public void beforeMethod(JoinPoint joinPoint) {
    log.debug("Before advice method in " + joinPoint.getSignature().getName() + " with arguments:");
    for(Object o:joinPoint.getArgs()){
      log.debug("\t - " + o);
    }
  }
}
    ]]></script>
  </div>
</div>

## Declarando mejores pointcuts

## Declarando aspectos con XML

