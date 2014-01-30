# Configuración del AppCtx

---

En Spring, los objetos no son responsables de encontrar o crear a otros objetos que necesitan para hacer su trabajo. En lugar de ello, se les dan referencias a los objetos con los que colaboran en el contenedor. Nuestro servicio de negocio por ejemplo, necesitará a su vez de otros componentes que les permita cumplir con el flujo del proceso deseado, y no pueden estar vacíos, sin refrenecia o nulos. 

El acto de crear dichas asociaciones entre los objetos de la aplicación es la esencia de la inyección de dependencias(DI), y es comúnmente referido como _wiring_. Como la inyección de dependencias es el concepto más elemental que hace Spring, mostraremos las técnicas mayormente usadas y las que encontrarás en la mayoría de los proyectos actualmente desarrollados.

## Declaración de beans

Como hemos dicho antes, Spring es un framework basado en contenedores. Pero si Spring no es configurado, entonces, tenemos un contenedor vacío y no sirve de mucho. Necesitamos configurar Spring para decirle que beans deberá contener y como los alambra para que trabajen juntos.

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

