# Configuración del AppCtx

---

En Spring, los objetos no son responsables de encontrar o crear a otros objetos que necesitan para hacer su trabajo. En lugar de ello, se les dan referencias a los objetos con los que colaboran en el contenedor. Nuestro servicio de negocio por ejemplo, necesitará a su vez de otros componentes que les permita cumplir con el flujo del proceso deseado, y no pueden estar vacíos, sin refrenecia o nulos. 

El acto de crear dichas asociaciones entre los objetos de la aplicación es la esencia de la inyección de dependencias(DI), y es comúnmente referido como _wiring_. Como la inyección de dependencias es el concepto más elemental que hace Spring, mostraremos las técnicas mayormente usadas y las que encontrarás en la mayoría de los proyectos actualmente desarrollados.

## Declaración de beans y configuración con XML

Como hemos dicho antes, Spring es un framework basado en contenedores. Pero si Spring no es configurado, entonces, tenemos un contenedor vacío y no sirve de mucho. Necesitamos configurar Spring para decirle que beans deberá contener y como los alambra para que trabajen juntos.

<div class="row">
  <div class="col-md-6">
    <h4><i class="icon-file"></i> UserServiceSimpleImpl.java</h4>
    <script type="syntaxhighlighter" class="brush: java"><![CDATA[
package com.makingdevs.practica4;

import com.makingdevs.model.User;
import com.makingdevs.services.UserService;

public class UserServiceSimpleImpl implements UserService {

  // Not implemented methods, yet...

}
    ]]></script>
  </div>
  <div class="col-md-6">
    <h4><i class="icon-file"></i> ApplicationContext.xml</h4>
    <script type="syntaxhighlighter" class="brush: xml"><![CDATA[
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
  
  <bean id="userService" class="com.makingdevs.practica4.UserServiceSimpleImpl">
  </bean>

</beans>
    ]]></script>
  </div>
</div>

<div class="row">
  <div class="col-md-12">
    <h4><i class="icon-file"></i> UseSpringAsLibraryTests.java</h4>
    <script type="syntaxhighlighter" class="brush: java"><![CDATA[
package com.makingdevs.practica4;

import static org.junit.Assert.*;

import org.junit.Test;
import org.springframework.beans.factory.BeanFactory;
import org.springframework.beans.factory.xml.XmlBeanFactory;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
import org.springframework.core.io.ClassPathResource;
import org.springframework.core.io.Resource;

import com.makingdevs.services.UserService;

public class UseSpringAsLibraryTests {

  @Test
  public void useSpringWithBeanFactory() {
    Resource resource = new ClassPathResource("com/makingdevs/practica4/ApplicationContext.xml");
    BeanFactory beanFactory = new XmlBeanFactory(resource);
    UserService userService = (UserService)beanFactory.getBean("userService");
    assertNotNull(userService);
  }
  
  @Test
  public void useSpringWithAppCtx() {
    ApplicationContext appCtx = new ClassPathXmlApplicationContext("com/makingdevs/practica4/ApplicationContext.xml");
    UserService userService = (UserService)appCtx.getBean("userService");
    assertNotNull(userService);
  }
  
  @Test
  public void useSpringWithAppCtxByType() {
    ApplicationContext appCtx = new ClassPathXmlApplicationContext("com/makingdevs/practica4/ApplicationContext.xml");
    UserService userService = appCtx.getBean(UserService.class);
    assertNotNull(userService);
  }

}
    ]]></script>
  </div>
</div>

## Inyección por constructor

<div class="alert alert-danger">
  <strong><i class="icon-terminal"></i> Ten cuidado...!</strong> Las clases de dominio no se benefician de la inyección de dependencias, los ejemplos mostrados a continuación no son prácticas recomendadas y sólo nos servirán de ejemplo.
</div>

<div class="row">
  <div class="col-md-6">
    <h4><i class="icon-file"></i> ConstructorInjectionAppCtx.xml</h4>
    <script type="syntaxhighlighter" class="brush: xml"><![CDATA[
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

  <bean id="projectFromConstructor" class="com.makingdevs.model.Project">
    <constructor-arg value="1"/>
    <constructor-arg value="My taskboards"/>
    <constructor-arg value="TASKBOARD"/>
    <constructor-arg value="Project description"/>
  </bean>
  
  <bean id="userFromConstructor" class="com.makingdevs.model.User">
    <constructor-arg value="makingdevs" index="1"/>
    <constructor-arg name="id" value="100"/>
    <constructor-arg value="true" type="boolean"/>
  </bean>

</beans>
    ]]></script>
  </div>
  <div class="col-md-6">
    <h4><i class="icon-file"></i> ConstructorInjectionTest.java</h4>
    <script type="syntaxhighlighter" class="brush: java"><![CDATA[
package com.makingdevs.practica5;

import static org.junit.Assert.assertNotNull;
import static org.junit.Assert.assertTrue;

import org.junit.Before;
import org.junit.Test;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

import com.makingdevs.model.Project;
import com.makingdevs.model.User;

public class ConstructorInjectionTest {
  
  private ApplicationContext appCtx;
  
  @Before
  public void setup(){
    // Look ma, String arrays!
    String[] configurations = {"com/makingdevs/practica5/ConstructorInjectionAppCtx.xml"};
    appCtx = new ClassPathXmlApplicationContext(configurations);
    assertNotNull(appCtx);
  }

  @Test
  public void getBeanWithConstructorInjection() {
    assertTrue(appCtx.containsBean("projectFromConstructor"));
    Project project = (Project)appCtx.getBean("projectFromConstructor");
    assertTrue(project.getId() == 1L);
    assertTrue(project.getName().equals("My taskboards"));
    assertTrue(project.getCodeName().equals("TASKBOARD"));
    assertTrue(project.getDescription().equals("Project description"));
  }
  
  @Test
  public void getAnotherBeanWithConstructor(){
    User user = appCtx.getBean(User.class);
    assertTrue(user.getId() == 100L);
    assertTrue(user.getUsername().equals("makingdevs"));
    assertTrue(user.isEnabled());
  }

}
    ]]></script>
  </div>
</div>

## Inyecciones por setter

<div class="alert alert-info">
  <strong><i class="icon-terminal"></i> Nuestra recomendación...!</strong> Usa mayormente la inyección por setters, pues es díficil de mantener una clase con un constructor y varios parámetros por inyectar.
</div>

<div class="row">
  <div class="col-md-6">
    <h4><i class="icon-file"></i> SetterInjectionAppCtx.xml</h4>
    <script type="syntaxhighlighter" class="brush: xml"><![CDATA[
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

  <bean id="projectFromConstructor" class="com.makingdevs.model.Project">
    <property name="id" value="1"/>
    <property name="name" value="My taskboards"/>
    <property name="codeName" value="TASKBOARD"/>
    <property name="description" value="Project description"/>
  </bean>
  
  <bean id="userFromConstructor" class="com.makingdevs.model.User">
    <property name="id" value="100"/>
    <property name="username">
      <value>makingdevs</value>
    </property>
    <property name="enabled" value="true"/>
    <property name="dateCreated">
      <bean class="java.util.Date" />
    </property>
  </bean>

</beans>
    ]]></script>
  </div>
  <div class="col-md-6">
    <h4><i class="icon-file"></i> SetterInjectionTest.java</h4>
    <script type="syntaxhighlighter" class="brush: java"><![CDATA[
package com.makingdevs.practica5;

import static org.junit.Assert.assertNotNull;
import static org.junit.Assert.assertTrue;

import org.junit.Before;
import org.junit.Test;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

import com.makingdevs.model.Project;
import com.makingdevs.model.User;

public class SetterInjectionTest {
  
  private ApplicationContext appCtx;
  
  @Before
  public void setup(){
    // Look ma! String array.
    String[] configurations = {"com/makingdevs/practica5/SetterInjectionAppCtx.xml"};
    appCtx = new ClassPathXmlApplicationContext(configurations);
    assertNotNull(appCtx);
  }

  @Test
  public void getBeanWithConstructorInjection() {
    assertTrue(appCtx.containsBean("projectFromConstructor"));
    Project project = (Project)appCtx.getBean("projectFromConstructor");
    assertTrue(project.getId() == 1L);
    assertTrue(project.getName().equals("My taskboards"));
    assertTrue(project.getCodeName().equals("TASKBOARD"));
    assertTrue(project.getDescription().equals("Project description"));
  }
  
  @Test
  public void getAnotherBeanWithConstructor(){
    User user = appCtx.getBean(User.class);
    assertTrue(user.getId() == 100L);
    assertTrue(user.getUsername().equals("makingdevs"));
    assertTrue(user.isEnabled());
    assertNotNull(user.getDateCreated());
  }

}
    ]]></script>
  </div>
</div>

## Inyección de colaboradores

<div class="alert alert-danger">
  <strong><i class="icon-terminal"></i> Ten cuidado...!</strong> Las clases de dominio no se benefician de la inyección de dependencias, los ejemplos mostrados a continuación no son prácticas recomendadas y sólo nos servirán de ejemplo.
</div>

<div class="row">
  <div class="col-md-6">
    <h4><i class="icon-file"></i> CollaboratorInjectionAppCtx.xml</h4>
    <script type="syntaxhighlighter" class="brush: xml"><![CDATA[
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

  <bean id="taskDescription1" class="java.lang.String">
    <constructor-arg value="Create schema" />
  </bean>

  <bean id="task1" class="com.makingdevs.model.Task">
    <property name="id" value="1" />
    <property name="description" ref="taskDescription1" />
    <property name="status">
      <value type="com.makingdevs.model.TaskStatus">
        TODO
      </value>
    </property>
    <property name="userStory" ref="userStory"/>
  </bean>

  <bean id="task2" class="com.makingdevs.model.Task">
    <property name="id" value="2" />
    <property name="description" value="Create folder structure" />
    <property name="status">
      <value type="com.makingdevs.model.TaskStatus">
        TODO
      </value>
    </property>
    <property name="userStory">
      <null/>
    </property>
  </bean>

</beans>
    ]]></script>
  </div>

  <div class="col-md-6">
    <h4><i class="icon-file"></i> AnotherCollaboratorInjectionAppCtx.xml</h4>
    <script type="syntaxhighlighter" class="brush: xml"><![CDATA[
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

  <bean id="userStory" class="com.makingdevs.model.UserStory">
    <property name="priority" value="1" />
    <property name="effort" value="3" />
    <property name="tasks">
      <array>
        <ref bean="task1" />
        <null />
        <ref bean="task2" />
        <bean id="task3" class="com.makingdevs.model.Task">
          <property name="id" value="3" />
          <property name="description" value="Initialize configuration" />
          <property name="status">
            <value type="com.makingdevs.model.TaskStatus">
              TODO
            </value>
          </property>
        </bean>
      </array>
    </property>
  </bean>

  <bean class="com.makingdevs.model.Project">
    <property name="codeName" value="TASKBOARD" />
    <property name="name" value="My Taskboard" />
    <property name="id" value="2" />
    <property name="userStories">
      <array>
        <ref bean="userStory" />
      </array>
    </property>
    <property name="participants">
      <set>
        <bean class="com.makingdevs.model.User">
          <property name="username" value="makingdevs" />
          <property name="enabled" value="true" />
          <property name="id" value="12" />
        </bean>
      </set>
    </property>
  </bean>

</beans>
    ]]></script>
  </div>
</div>

<div class="row">
  <div class="col-md-12">
    <h4><i class="icon-file"></i> CollaboratorInjectionTest.java</h4>
    <script type="syntaxhighlighter" class="brush: java"><![CDATA[
package com.makingdevs.practica6;

import static org.junit.Assert.assertNotNull;
import static org.junit.Assert.assertTrue;

import org.junit.Before;
import org.junit.Test;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

import com.makingdevs.model.Project;
import com.makingdevs.model.User;
import com.makingdevs.model.UserStory;

public class CollaboratorInjectionTest {
  
  private ApplicationContext appCtx;

  @Before
  public void setup(){
    String[] configurations = {
        "com/makingdevs/practica6/CollaboratorInjectionAppCtx.xml",
        "com/makingdevs/practica6/AnotherCollaboratorInjectionAppCtx.xml"
        };
    appCtx = new ClassPathXmlApplicationContext(configurations);
    assertNotNull(appCtx);
  }

  @Test
  public void getBeanWithDependencies() {
    Project project = appCtx.getBean(Project.class);
    assertTrue(project.getId() == 2L);
    assertTrue(project.getCodeName().equals("TASKBOARD"));
    assertTrue(project.getUserStories().size() == 1);
    assertTrue(project.getParticipants().size() == 1);
    User user = project.getParticipants().get(0);
    assertTrue(user.getUsername().equals("makingdevs"));
    UserStory userStory = project.getUserStories().get(0);
    assertTrue(userStory.getEffort() == 3);
    assertTrue(userStory.getTasks().size() == 4);
    assertTrue(userStory.getTasks().contains(null));
    // Wherever you want...
  }

}
    ]]></script>
  </div>
</div>

------

<div cxlass="row">
  <div class="col-md-6">
    <h4><i class="icon-file"></i> MultiPropertiesBean.java</h4>
    <script type="syntaxhighlighter" class="brush: java"><![CDATA[
package com.makingdevs.practica6;

import java.util.List;
import java.util.Map;
import java.util.Properties;
import java.util.Set;

public class MultiPropertiesBean {
  
  private Map<String, Integer> aMap;
  private List<String> multiLine;
  private Set<Float> primeNumbers;
  private Properties courseProperties;
  
  public Properties getCourseProperties() {
    return courseProperties;
  }
  public void setCourseProperties(Properties courseProperties) {
    this.courseProperties = courseProperties;
  }
  public Map<String, Integer> getaMap() {
    return aMap;
  }
  public void setaMap(Map<String, Integer> aMap) {
    this.aMap = aMap;
  }
  public List<String> getMultiLine() {
    return multiLine;
  }
  public void setMultiLine(List<String> multiLine) {
    this.multiLine = multiLine;
  }
  public Set<Float> getPrimeNumbers() {
    return primeNumbers;
  }
  public void setPrimeNumbers(Set<Float> primeNumbers) {
    this.primeNumbers = primeNumbers;
  }
  
}
    ]]></script>
  </div>
  <div class="col-md-6">
    <h4><i class="icon-file"></i> MoreInjectedBeansAppCtx.xml</h4>
    <script type="syntaxhighlighter" class="brush: xml"><![CDATA[
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
  
  <bean id="tres" class="java.lang.Integer">
   <constructor-arg value="3"/>
  </bean>

  <bean id="multiPropertiesBean" class="com.makingdevs.practica6.MultiPropertiesBean">
    <property name="aMap">
      <map>
        <entry key="Uno"><value>1</value></entry>
        <entry key="Dos" value="2"></entry>
        <entry key="Uno" value-ref="tres"/>
        <entry key="Tres" value-ref="tres"/>
      </map>
    </property>
    <property name="multiLine">
      <array>
        <value>Welcome!!!</value>
        <value>You're MakingDevs...</value>
        <value>And you're here because...</value>
        <value>You want to be a better developer!</value>
      </array>
    </property>
    <property name="primeNumbers">
      <set>
        <value>1</value>
        <value>3</value>
        <value>5</value>
        <value>7</value>
        <value>11</value>
        <value>13</value>
      </set>
    </property>
    <property name="courseProperties">
      <props>
        <prop key="SPRING-ESSENTIALS">Diseño de aplicaciones con Spring</prop>
        <prop key="SPRING-DATA_ACCESS">Acceso a datos con Spring</prop>
        <prop key="SPRING-WEB">Desarrollo Web con Spring</prop>
      </props>
    </property>
  </bean>

</beans>
    ]]></script>
  </div>
</div>

<div cxlass="row">
  <div class="col-md-12">
    <h4><i class="icon-file"></i> MultiPropertiesCollaboratorInjectionTest.java</h4>
    <script type="syntaxhighlighter" class="brush: java"><![CDATA[
package com.makingdevs.practica6;

import static org.junit.Assert.assertNotNull;
import static org.junit.Assert.assertTrue;

import org.junit.Before;
import org.junit.Test;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class MultiPropertiesCollaboratorInjectionTest {

  private ApplicationContext appCtx;

  @Before
  public void setup() {
    String[] configurations = { "com/makingdevs/practica6/MoreInjectedBeansAppCtx.xml" };
    appCtx = new ClassPathXmlApplicationContext(configurations);
    assertNotNull(appCtx);
  }

  @Test
  public void getBeanWitMultiProperties() {
    MultiPropertiesBean multi = appCtx.getBean(MultiPropertiesBean.class);
    assertTrue(multi.getaMap().size() == 3);
    assertTrue(multi.getaMap().containsKey("Uno"));
    assertTrue(multi.getMultiLine().size() == 4);
    assertTrue(multi.getPrimeNumbers().size() == 6);
    assertTrue(multi.getCourseProperties().size() == 3);
    assertTrue(multi.getCourseProperties().get("SPRING-WEB").equals("Desarrollo Web con Spring"));
    // Wherever you want...
  }

}
    ]]></script>
  </div>
</div>

### Proceso de resolución de dependencias

* El `ApplicationContext` es creado e inicializado con la configuración de los metadatos que describe todos los beans. Los metadatos pueden ser XML, Java, Groovy o anotaciones.
* Para cada _bean_, sus dependencias son expresadas en forma de propiedades, argumentos del constructor, o argumentos de un método de factoría estática si se está usando en lugar de un constructor. Dichas dependencias son proveídas al bean, cuando el bean es creado.
* Cada propiedad o argumento del constructor es una definición actual del valor a establecer, o una referencia a otro bean en el contenedor.
* Cada propiedad o argumento del constructor el cual es un valor es convertido de su formato específico al tipo actual de la propiedad o argumento del constructor. Por default Spring trbajá muy bien con los tipos más simples que tenemos en la plataforma Java.

El contenedor de Spring valida la configuración de cada bean al momento de que se va creando el contenedor, incluyendo las referencias que tiene un bean hacia otros beans. Sin embargo, las propiedades de los beans por si mismas no son establecidas hasta que hayan sido creadas.

### Namespaces 

* `aop` Provee elementos para declarar aspectos  y para automáticamente proxear clases anotadas con AspectJ como aspectos de Spring
* `beans` El namespace central de Spring, habilita la declaración de beans y como deben ser alambrados. 
* `context` Viene con elementos para configurar el application context de Spring, incluyendo la habilidad para autodetectar y auto-alambrar beans y la inyección de los objetos no directamente manejados por Spring.
* `jee` Ofrece integración con la API de JEE como JNDI y EJB.
* `jms` Provee de elementos de configuración para declarar messgae-driven POJO's.
* `lang` Habilita la declaración de beansque implementan Groovy, JRuby o scripts de BeanShell.
* `mvc` Habilita las capacidades de Spring MVC como las anotaciones orientadas a controllers, vistas e interceptores.
* `oxm` Soporta la configuración para las características del mapeo objeto a XML(object-to-XML).
* `tx` Provee de configuración para transacciones declarativas.
* `util` Una variedad de selección de elementos de utilería. Incluye la habilidad de declarar colecciones como beans y soporte para elementos marcadores de propiedades.
* Hay algunos otros más...

<div class="row">
  <div class="col-md-6">
    <h4><i class="icon-file"></i> UsingNamespacesAppCtx.xml</h4>
    <script type="syntaxhighlighter" class="brush: xml"><![CDATA[
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:util="http://www.springframework.org/schema/util"
  xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
    http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util-4.0.xsd">

  <util:properties id="courseProperties" location="com/makingdevs/practica7/externalProperties.properties"/>

  <bean id="tres" class="java.lang.Integer">
    <constructor-arg value="3" />
  </bean>

  <util:map id="naturalNumbers">
    <entry key="Uno">
      <value>1</value>
    </entry>
    <entry key="Dos" value="2"></entry>
    <entry key="Uno" value-ref="tres" />
    <entry key="Tres" value-ref="tres" />
  </util:map>

  <util:list id="multiLine">
    <value>Welcome!!!</value>
    <value>You're MakingDevs...</value>
    <value>And you're here because...</value>
    <value>You want to be a better developer!</value>
  </util:list>

  <util:set id="primeNumbers">
    <value>1</value>
    <value>3</value>
    <value>5</value>
    <value>7</value>
    <value>11</value>
    <value>13</value>
  </util:set>

  <bean id="multiPropertiesBean" class="com.makingdevs.practica6.MultiPropertiesBean">
    <property name="aMap" ref="naturalNumbers"/>
    <property name="multiLine" ref="multiLine"/>
    <property name="primeNumbers" ref="primeNumbers"/>
    <property name="courseProperties" ref="courseProperties"/>
  </bean>

</beans>
    ]]></script>
  </div>
  <div class="col-md-6">
    <h4><i class="icon-file"></i> MultiPropertiesWithNamespaceCollaboratorInjectionTest.xml</h4>
    <script type="syntaxhighlighter" class="brush: java"><![CDATA[
package com.makingdevs.practica7;

import static org.junit.Assert.assertNotNull;
import static org.junit.Assert.assertTrue;

import org.junit.Before;
import org.junit.Test;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

import com.makingdevs.practica6.MultiPropertiesBean;

public class MultiPropertiesWithNamespaceCollaboratorInjectionTest {

  private ApplicationContext appCtx;

  @Before
  public void setup() {
    String[] configurations = { "com/makingdevs/practica7/UsingNamespacesAppCtx.xml" };
    appCtx = new ClassPathXmlApplicationContext(configurations);
    assertNotNull(appCtx);
  }

  @Test
  public void getBeanWitMultiProperties() {
    MultiPropertiesBean multi = appCtx.getBean(MultiPropertiesBean.class);
    assertTrue(multi.getaMap().size() == 3);
    assertTrue(multi.getaMap().containsKey("Uno"));
    assertTrue(multi.getMultiLine().size() == 4);
    assertTrue(multi.getPrimeNumbers().size() == 6);
    assertTrue(multi.getCourseProperties().size() == 3);
    assertTrue(multi.getCourseProperties().get("SPRING-WEB").equals("Desarrollo Web con Spring"));
    // Wherever you want...
  }

}
    ]]></script>
  </div>
</div>

------

### Alcance de los beans y modelos de instanciación

<blockquote><p>Por default, todos los beans son singletons.</p></blockquote>

Cuando el contenedor despacha un bean, siempre manejará la misma instancia del bean. Pero habrá veces en las que tal vez necesites una nueva instancia del bean cada vez que lo pidas. Cuando declaras un `<bean>` en Spring tienes la opción de definir el alcance del mismo. Entre los alcances que tenemos disponibles podemos mencionar los siguientes:

* `singleton` Alcance para la definición de un bean para una sola instancia por contenedor de Spring.
* `prototype` Permite a un bean ser instanciado cualqueir número de veces(una vez por uso).
* `request` Alcance para la definición de un bean en una solicitud HTTP. Sólo válido con el uso de SpringMVC.
* `session` Alcance de un dentro de la definición de una sesión HTTP.

Para la mayoría de las veces, probablemente será suficiente con dejar el alcance como _singleton_, sin embargo _prototype_ será útil en situaciones donde quieras usar Spring como una fábrica para instancias de objetos de dominio nuevos.

## Configuración con Anotaciones


## Configuración con JavaConfig


## Configuración con Groovy


## Spring Expression Language



Nota: Bean Scoping, inicializando y destruyendo beans