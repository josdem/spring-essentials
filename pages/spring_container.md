# El conteneder de Spring

------

## Bill of materials

Si estás usando Maven para la administración de dependencias no necesitas proveerlas de forma explícita. Por ejemplo, para crear una aplicación con Spring usando sólo inyección de dependencias la dependencia de Maven que necesitas es:

<div class="row">
  <div class="col-md-12">
    <h4><i class="icon-file"></i> pom.xml</h4>
    <script type="syntaxhighlighter" class="brush: xml"><![CDATA[
      <dependencies>
        <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-context</artifactId>
          <version>4.0.0.RELEASE</version>
        </dependency>
      </dependencies>
    </script>
  </div>
</div>

El ejemplo anterior trabaja con el repo Central de Maven. Pero para garantizar la entrega de las dependencias podemos usar alguno de los repositorios de Spring, sólo necesitamos agregarlos a nuestra configuración.

<div class="row">
  <div class="col-md-4">
    <h4><i class="icon-file"></i> pom.xml - RELEASES</h4>
    <script type="syntaxhighlighter" class="brush: xml"><![CDATA[
<repositories>
  <repository>
    <id>io.spring.repo.maven.release</id>
    <url>http://repo.spring.io/release/</url>
    <snapshots>
      <enabled>false</enabled>
    </snapshots>
  </repository>
</repositories>
    </script>
  </div>
  <div class="col-md-4">
    <h4><i class="icon-file"></i> pom.xml - MILESTONES</h4>
    <script type="syntaxhighlighter" class="brush: xml"><![CDATA[
      <repositories>
        <repository>
          <id>io.spring.repo.maven.milestone</id>
          <url>http://repo.spring.io/milestone/</url>
          <snapshots>
            <enabled>false</enabled>
          </snapshots>
        </repository>
      </repositories>
    </script>
  </div>
  <div class="col-md-4">
    <h4><i class="icon-file"></i> pom.xml - SNAPSHOTS</h4>
    <script type="syntaxhighlighter" class="brush: xml"><![CDATA[
    <repositories>
      <repository>
        <id>io.spring.repo.maven.snapshot</id>
        <url>http://repo.spring.io/snapshot/</url>
        <snapshots>
          <enabled>true</enabled>
        </snapshots>
      </repository>
    </repositories>
    </script>
  </div>
</div>

Es posible accidentalmente mezclar diferentes versiones de los JAR's de Spring cuando se usa Maven. Por ejemplo, podrás encontrar librerías de terceros que usan versiones específicas de Spring, o incluso algún subproyecto de Spring usa ciertas versiones del framework, jalando por lo tanto versiones anteriores por Dependencias Transitivas.

Para solucionar el problema, Maven soporta el concepto de "Bill of materials". Puedes importar la dependencia `spring-framework-bom` en la section de `dependencyManagement` para asegurarse que las dependencias de Spring(directas y transitivas) son de la misma versión.

<div class="row">
  <div class="col-md-6">
    <h4><i class="icon-file"></i> pom.xml</h4>
    <script type="syntaxhighlighter" class="brush: xml"><![CDATA[
      <dependencyManagement>
        <dependencies>
          <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-framework-bom</artifactId>
            <version>4.0.0.RELEASE</version>
            <type>pom</type>
            <scope>import</scope>
          </dependency>
        </dependencies>
      </dependencyManagement>
    </script>
  </div>
  <div class="col-md-6">
    <h4><i class="icon-file"></i> pom.xml</h4>
    <script type="syntaxhighlighter" class="brush: xml"><![CDATA[
      <dependencies>
        <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-context</artifactId>
        </dependency>
        <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-web</artifactId>
        </dependency>
      <dependencies>
    </script>
  </div>
</div>

### Administración de dependencias con Gradle

Para usar los repositorios de Spring con Gradle incluye las URL's apropiadas en la sección de repositorios, y agrega las dependencias a tu gusto:

<div class="row">
  <div class="col-md-12">
    <h4><i class="icon-file"></i> build.gradle</h4>
    <script type="syntaxhighlighter" class="brush: groovy"><![CDATA[
      repositories {
        mavenCentral()
        maven { url "http://repo.spring.io/release" }  // milestone or snapshotsl
      }
      dependencies {
        compile("org.springframework:spring-context:4.0.0.RELEASE")
        testCompile("org.springframework:spring-test:4.0.0.RELEASE")
      }
    </script>
  </div>
</div>


## BeanFactory y AppCtx

<blockquote>
  <p>
    En Spring los objetos que forman la columna vertebral de tu aplicación y que son manejados por el contenedor de IoC son llamados <strong>beans</strong>.
  </p>
</blockquote>

Spring provee dos tipos de implementación del contenedor de IoC. La básica es llamada _bean factory_, la más avanzada es llamada _application context_, el cual es una extensión compatible con el _bean factory_. Y la configuración para estos dos tipos de contenedores es idéntica. El _AppCtx_ provee funcionalidades más avanzadas pero se mantiene simple de usar. Es más recomemdable usar el _AppCtx_ para cada aplicación a menos que los recursos de esta sean restringidos, como cuando corremos un appleto o un dispositvo móvil. Dichos contenedores están declarados en las siguientes interfaces:

* [org.springframework.beans.factory.BeanFactory](http://docs.spring.io/spring/docs/4.0.x/javadoc-api/org/springframework/beans/factory/BeanFactory.html)
* [org.springframework.context.ApplicationContext](http://docs.spring.io/spring/docs/4.0.x/javadoc-api/org/springframework/context/ApplicationContext.html)

En donde, como se puede ver el AppCtx es un subtipo de BeanFactory, y mas interesante es, ver los tipos de ApplicationContext con los que contamos para crear una aplicación, entre ellos podemos nombrar de forma destacada:

* AnnotationConfigApplicationContext
* AnnotationConfigWebApplicationContext
* ClassPathXmlApplicationContext
* FileSystemXmlApplicationContext
* GenericGroovyApplicationContext
* GenericWebApplicationContext
* GenericXmlApplicationContext
* XmlWebApplicationContext

<div class="row">
  <div class="col-md-12">
    <h4><i class="icon-code"></i> Instanciando el AppCtx</h4>
    <script type="syntaxhighlighter" class="brush: java"><![CDATA[
      ApplicationContext context = new FileSystemXmlApplicationContext("/tmp/foo.xml");
      ApplicationContext context2 = new ClassPathXmlApplicationContext("/tmp/bar.xml");
    </script>
  </div>
</div>

**Adicionalmente, es bueno mencionar que los paquetes `org.springframework.beans` y `org.springframework.context` son la base del contenedor de Spring.**

### Diferencias entre el BeanFactory y el AppCtx

El `BeanFactory` proporciona la base fundamental para la funcionalidad del contenedor de IoC de Spring pero sólo se usa directamente en la integración con otros frameworks de terceros, y ahora es en gran parte de naturaleza histórica para la mayoría de los usuarios de Spring. Sin embargo la regla es: **Usa un `ApplicationContext` a menos que tengas una buena razón para no hacerlo.**

El `ApplicationContext` agrega la integración con características de AOP, manejo de recursos, publicación de eventos y contextos específicos en función del tipo de aplicación.

* Un BeanFactory
    * Instancia y alambra los beans
* Un ApplicationContext
    * Instancia y alambra los beans
    * Hace un registro automático con `BeanPostProcessor`
    * Hace un registro automático  del `BeanFactoryPostProcessor`
    * Habilita el acceso conveniente al `MessageSource`
    * Hace la publicación del `ApplicationEvent`

<div class="row">
  <div class="col-md-12">
    <h4><i class="icon-code"></i> Usando el BeanFactory</h4>
    <script type="syntaxhighlighter" class="brush: java"><![CDATA[
      XmlBeanFactory factory = new XmlBeanFactory(new FileSystemResource("beans.xml"));
      MyBeanPostProcessor postProcessor = new MyBeanPostProcessor();
      factory.addBeanPostProcessor(postProcessor);
      PropertyPlaceholderConfigurer cfg = new PropertyPlaceholderConfigurer();
      cfg.setLocation(new FileSystemResource("jdbc.properties"));
      cfg.postProcessBeanFactory(factory);
    </script>
  </div>
</div>

<div class="row">
  <div class="col-md-12">
    <h4><i class="icon-code"></i> Archivo base de configuración: appctx.xml</h4>
    <script type="syntaxhighlighter" class="brush: xml"><![CDATA[
    <?xml version="1.0" encoding="UTF-8"?>
      <beans xmlns="http://www.springframework.org/schema/beans"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xsi:schemaLocation="http://www.springframework.org/schema/beans
      http://www.springframework.org/schema/beans/spring-beans.xsd">
        <bean id="..." class="...">
          <!-- colaboradores y configuraciones de este bean -->
        </bean>
    </beans>
    </script>
  </div>
</div>

## Inyección de Dependencias entre beans con elementos transversales.

**IMAGEN**

## Ciclo de vida de los beans

<blockquote>
  <p>
    Un bean es un objeto que es instanciado, ensamblado, y de alguna manera administrado por el contenedor de Spring.
  </p>
</blockquote>

En una aplicación basada en Spring, los objetos de la aplicación vivirán dentro del contenedor de IoC, este último los creará y ellos se alambrarán, se configurarán y el mismo contenedor los administrará.

![Alt container-magic](img/container-magic.png "Container Magic")

El contenedor es la parte central de SpringFramework, el cual, usa inyección de dependencias para administrar los componentes de la aplicación. Esto incluye la creación de asociaciones entre componentes colaboradores.

En una aplicación Java tradicional el ciclo de vida de un bean es simple, la palabra reservada `new` es usada para instanciarlo y con eso esta listo para usarse. Una vez que ya no se usa más, entonces es candidato para que el Garbage Collector pase por él. En contraste, el ciclo de vida de un bean dentro del contenedor de Spring es más elaborado. Como se pudo apreciar anteriormente, el `BeanFactory` ejecuta varias pasos antes de enlistar un bean, y sumado con lo que hace el `ApplicationContext` podemos enlistar las siguientes:

1. Spring instancia el bean
2. Spring inyecta valores y referencias de beans en sus propiedades.
3. Si el bean implementa `BeanNameAware`, Spring pasa el ID del bean al método `setBeanName()`.
4. Si el bean implementa `BeanFactoryAware`, Spring llama al método `setBeanFactory()`, pasando el bean a dicho factory.
5. Si el bean implementa `ApplicationContextAware`, Spring llama el método `setApplicationContext()`, pasando la referencia a dicho AppCtx dentro del bean.
6. Si cualquiera de los beans implementa la interface `BeanPostProcessor`, Spring llama a su método `postProcessBeforeInitialization()`.
7. Si cualquiera de los beans implementa la interfaz `InitializingBean`, Spring llama a su método `agterPropertiesSet()`. Similarmente, si el bean fue declarado con un _init-method_, entonces dicho método es llamado.
8. Si existen beans que implementan `BeanPostProcessor`, Soring llmará a su método `postProcessAfterInitialization()`.
9. En este punto, el bean esta listo para ser usado por la aplicación y permanecerá en el contexto de la aplicación hasta que dicho contexto sea destruido.
10. Si cualquier bean implementa la interfaz `DisposableBean`, entonces Spring llamará a su método `destroy()`. De otra forma, si cualquier bean fue declarado con un _destroy-method_, entonces dicho método será llamado.

## Caso de estudio

Nuestro ejemplo estará basado en un tablero de tareas(Taskboard), el cual esta asignado a algun proyecto que a su vez tiene varias historias de usuario, dichas historias serán pobladas por las tareas. Todo este conjunto nos dará como resultado un tablero que potencialmente podrá ser visualizado en un front-end.

<div class="row">
  <div class="col-md-4">
    <h4><i class="icon-file"></i> Project.java</h4>
    <script type="syntaxhighlighter" class="brush: java"><![CDATA[
package com.makingdevs.container;

import java.util.Date;
import java.util.List;

public class Project {
  private String name;
  private Date dateCreated;
  private Date lastUpdated;
  
  private List<UserStory> userStories;
  
  // Getters y Setters
}
    </script>
  </div>
  <div class="col-md-4">
    <h4><i class="icon-file"></i> UserStory.java</h4>
    <script type="syntaxhighlighter" class="brush: java"><![CDATA[
package com.makingdevs.container;

import java.util.Date;
import java.util.List;

public class UserStory {
  private String asUser;
  private String iWant;
  private String because;
  private Date dateCreated;
  private Date lastUpdated;
  
  private Project project;
  private List<Task> tasks;
  // Getters y Setters
}
    </script>
  </div>
  <div class="col-md-4">
    <h4><i class="icon-file"></i> Task.java</h4>
    <script type="syntaxhighlighter" class="brush: java"><![CDATA[
package com.makingdevs.container;

import java.util.Date;
import java.util.List;

public class Task {
  private String description;
  private TaskStatus status;
  private Date dateCreated;
  private Date lastUpdated;
  
  private UserStory userStory;
  private List<User> participants;
  // Getters y Setters
}
    </script>
  </div>
</div>

<div class="row">
  <div class="col-md-6">
    <h4><i class="icon-file"></i> TaskStatus.java</h4>
    <script type="syntaxhighlighter" class="brush: java"><![CDATA[
package com.makingdevs.container;

public enum TaskStatus {
  TODO,WIP,DONE;
}
    </script>
  </div>
  <div class="col-md-6">
    <h4><i class="icon-file"></i> User.java</h4>
    <script type="syntaxhighlighter" class="brush: java"><![CDATA[
package com.makingdevs.container;

import java.util.Date;

public class User {
  private String username;
  private Date dateCreated;
  private Date lastUpdated;
  // Getters y Setters
}
    </script>
  </div>
</div>

