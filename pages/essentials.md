# Elementos esenciales de Springframework

------

Spring es un framework Open Source, originalmente creado por Rod Johnson y descrito en su libro [_Expert One-on-one: J2EE Design and Development_](http://www.wrox.com/WileyCDA/WroxTitle/productCd-0764543857.html). Spring fue creado para direccionar la complejidad del desarrollo de una aplicación empresarial, y hace posible usar JavaBeans para mantener la simplicidad del desarrollo, así como, la creación de componentes complejos con conceptos muy simples como son Clases e Interfaces. Y aunque es mayormente usado en aplicaciones cliente-servidor, cualquier tipo de aplicación puede beneficiarse de Spring.

<blockquote>
Spring simplifica de sobremanera el desarrollo Java.
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

* Spring 3.0
    * Soporte a gran escala para REST en SpringMVC, incluyendo los controllers que responden a las URL's del estilo REST con XML, JSON, RSS o cualquier otra respuesta adecuada.
    * Un nuevo lenguaje de expresión que ofrece inyección de dependencias a un nuevo nivel habilitando la inyección de valores de una variedad de fuentes, incluyendo otros beans y propiedades de sistema.
    * Nuevas anotaciones para SpringMVC, incluyendo `@CookieValue` y `@Request-Header`, para jalar valores desde la información del navegador y el request.
    * Un nuevo namespace XML para una configuración más fácil de SpringMVC
    * Soporte para validaciones declarativas con anotaciones de JSR-303(Bean Validations).
    * Soporte para la especificación JSR-330 en inyección de dependencias.
    * Declaraciones basada en anotaciones de métodos calendarizados y asíncronos.
    * Una nueva configuración basada en anotaciones para librar a Spring de la configuración XML.
    * La funcionalidad de mapeo Objeto a XML del proyecto de Spring WebServices(OXM).
    * Ya no hay soporte para Java 1.4

El pasado Diciembre del 2013 se libero la versión 4 de Spring, con cambios y mejoras muy notables de mencionar, y aún mejor conocer, ya que se adaptan a las nuevas necesidades de las aplicaciones modernas:

* Spring 4
    * Esta versión es compatible con Java 1.6+.
    * Nuevas versiones de integración con frameworks como: Hibernate 3.6+, EhCache 2.1+, Quartz 1.8+, Groovy 1.8+, y Joda-Time 2.0+, Hibernate Validator 4.3+ y Jackson 2.0+.
    * Un POM llamado **Bill of Materials** para que los usuarios de Maven no conflictuen las versiones.
    * El soporte temprano para Java 8
    * Configuración del AppCtx con ayuda el lenguaje dinámico Groovy, demasiado interesante, pues provee de un DSL para hacerlo.
    * Soporte para Servlet 3.0.
    * Más soporte para REST a través de `@RestController` y el `AsyncRestTemplate`.
    * El nuevo soporte para WebSockets compatible con el JSR-356, en conjunto con [SockJS](https://github.com/sockjs/sockjs-client), a través de STOMP.
    * Mejor soporte de Tesing con el uso de meta-anotaciones, el uso de profiles con `@ActiveProfiles`.
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