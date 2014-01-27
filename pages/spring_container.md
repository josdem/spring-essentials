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
  <div class="col-md-6">
    <h4><i class="icon-file"></i> build.gradle</h4>
    <script type="syntaxhighlighter" class="brush: groovy"><![CDATA[
      repositories {
        mavenCentral()
        maven { url "http://repo.spring.io/release" }  // milestone or snapshots
      }
      dependencies {
        compile("org.springframework:spring-context:4.0.0.RELEASE")
        testCompile("org.springframework:spring-test:4.0.0.RELEASE")
      }
    </script>
  </div>
</div>


## AppCtx y BeanFactory


## Ciclo de vida de los beans


## Declaración de beans


## Inyección por constructor


## Inyecciones por setter


## Wiring

