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

## Módulos de Spring

Springframework esta formado en alrededor de 20 módulos. Dichos módulos están agrupados dentro del Core Container, la Integración y el Acceso a Datos, Web, AOP, la Instrumentación y Test.

![Alt spring-overview](img/spring-overview.png "Spring Overvivew")

## Diseño de aplicaciones(orientada a interfaces)
