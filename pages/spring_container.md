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

**Adicionalmente, es bueno mencionar que los paquetes _org.springframework.beans_ y _org.springframework.context_ son la base del contenedor de Spring.**

### Diferencias entre el BeanFactory y el AppCtx

El `BeanFactory` proporciona la base fundamental para la funcionalidad del contenedor de IoC de Spring pero sólo se usa directamente en la integración con otros frameworks de terceros, y ahora es en gran parte de naturaleza histórica para la mayoría de los usuarios de Spring. Sin embargo la regla es: **Usa un `ApplicationContext` a menos que tengas una buena razón para no hacerlo.**

El `ApplicationContext` agrega la integración con características de AOP, manejode recursos, publicación de eventos y contextos específicos en función del tipo de aplicación.

* Un BeanFactory
    * Instancia y alambra los beans
* Un ApplicationContext
    * Instancia y alambra los beans
    * Hace un registro automático con `BeanPostProcessor`
    * Hace un registro automático  del `BeanFactoryPostProcessor`
    * Habilita el acceso conveniente al `MessageSource`
    * Hace la publicación del `ApplicationEvent`

#### ¿Qué hace el BeanFactory?

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

## Ciclo de vida de los beans

<blockquote>
  <p>
    Un bean es un objeto que es instanciado, ensamblado, y de alguna manera administrado por el contenedor de Spring.
  </p>
</blockquote>


## Declaración de beans


## Inyección por constructor


## Inyecciones por setter


## Wiring

