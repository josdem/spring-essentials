# Configuración del AppCtx

---

En Spring, los objetos no son responsables de encontrar o crear a otros objetos que necesitan para hacer su trabajo. En lugar de ello, se les dan referencias a los objetos con los que colaboran en el contenedor. Nuestro servicio de negocio por ejemplo, necesitará a su vez de otros componentes que les permita cumplir con el flujo del proceso deseado, y no pueden estar vacíos, sin refrenecia o nulos. 

El acto de crear dichas asociaciones entre los objetos de la aplicación es la esencia de la inyección de dependencias(DI), y es comúnmente referido como _wiring_. Como la inyección de dependencias es el concepto más elemental que hace Spring, mostraremos las técnicas mayormente usadas y las que encontrarás en la mayoría de los proyectos actualmente desarrollados.

## Declaración de beans

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
    </script>
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
    </script>
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
    </script>
  </div>
</div>

## Configuración con XML


## Inyección por constructor

<div class="alert alert-danger">
  <strong><i class="icon-terminal"></i> Ten cuidado...!</strong> Las clases de dominio no se benefician de la inyección de dependencias, los ejemplos mostrados a continuación no son prácticas recomendadas y sólo nos servirán de ejemplo.
</div>




## Inyecciones por setter

<div class="alert alert-info">
  <strong><i class="icon-terminal"></i> Nuestra recomendación...!</strong> Usa mayormente la inyección por setters, pues es díficil de mantener una clase con un constructor y varios parámetros por inyectar.
</div>


### Namespaces 


## Configuración con Anotaciones


## Configuración con JavaConfig


## Configuración con Groovy


## Spring Expression Language

