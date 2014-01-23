# Inversion of control y Dependency Injection

------

## Inversión de control

De forma conceptual, _Inversion of Control_ es el principio donde el control del flujo de un programa es invertido, es decir, generalmente nosotros le indicamos(de manera secuencial y procedural) la forma en como los objetos se crean y asocian, además de como se comunican. Usando la inversión de control nosotros indicamos a través de algún elemento(framework, servicios, metadatos) que tome el control de la creación de los componentes y de las llamadas a los mismos.

IoC es parte fundamental que hace a un framework distinto de una librería. Una librería es esencialmente un conjunto de funciones que se pueden invocar, usualmente organizados en clases y paquetes. Cada llamada hace algo y regresa el control al cliiente..

Un ejemplo muy simple sería como el siguiente:

```
print "Enter yout name:"
read name
print "Enter your birthday:"
read birthday
store in database
```

Ahora vamos a invertir el control:

```
when the user types in field input_name, store it in NAME
when the user types in field input_address, store it in ADDRESS
when the user saves action, call StoreUserInfoInDB
```

Ahora el control está invertido, básicamente, cualquier cosa con un loop en el evento, los callbacks o triggers pueden beneficiarse del IoC. Springframework está basado en callbacks.

En términos más llanos para nuestro caso significa que en lugar de propiamente llamar a los métodos del framework, el framework llamará a las implementaciones proveídas por la aplicación.

<div class="row">
  <div class="col-md-6">
    <h4><i class="icon-file"></i> TaskManagerHighTied.java</h4>
    <script type="syntaxhighlighter" class="brush: java"><![CDATA[
package com.makingdevs.ioc;

import com.makingdevs.essentials.TaskStore;

public class TaskManagerHighTied {
  private TaskStore taskStore;
  private UserStore userStore;
  private LineStore lineStore;

  public TaskManagerHighTied(){
    this.taskStore = new TaskStore();
    this.userStore = new UserStore();
    this.lineStore = new Store();
  }
  //..
}
    ]]></script>
  </div>
  <div class="col-md-6">
    <h4><i class="icon-file"></i> TaskManagerLowTied.java</h4>
    <script type="syntaxhighlighter" class="brush: java"><![CDATA[
package com.makingdevs.ioc;

import com.makingdevs.essentials.TaskStore;

public class TaskManagerLowTied {
  private TaskStore taskStore;
  private UserStore userStore;
  private LineStore lineStore;

  public TaskManagerHighTied(TaskStore taskStore,
                             UserStore userStore,
                             LineStore lineStore) {
    this.taskStore = taskStore;
    this.userStore = userStore;
    this.lineStore = lineStore;
  }
}
    ]]></script>
  </div>
</div>

<div class="bs-callout bs-callout-warning">
<h4><i class="icon-coffee"></i> Información de utilidad</h4>
  <p>
  Y aunque no es la mejor forma de hacerlo ejemplifica perfectamente la inversión de control que deseamos hacer.
  </p>
</div>

### Estos principios de diseño debes aprender.

#### Acerca de las interdependencias

<blockquote>
  <p>Módulos de alto nivel NO deberían depender de módulos de bajo nivel. AMBOS deben depender de abstracciones.</p>
  <small>Uncle Bob Martín - <cite title="Clean Code">Clean Code</cite></small>
</blockquote>

#### Acerca de las implementaciones

<blockquote>
  <p>Las abstracciones no deben depender de los detalles. Los detalles deben depender de las abstracciones.</p>
  <small>Uncle Bob Martín - <cite title="Clean Code">Clean Code</cite></small>
</blockquote>

## Inyección de dependencias

DI o Dependency Injection es una forma de Inversión de Control, donde las implementaciones son pasadas a un objeto a través de constructores, setters, o búsqueda de servicios(lookups), de las cuales el objeto _dependerá_ para comportarse correctamente.

Aunque se puede usar IoC sin DI, a esto se le conoce como el patrón Template por que la implementación puede ser cambiada a través de subclases. Así también, Los frameworks de DI están diseñados para hacer uso de la técnica y pueden definir interfaces para facilitar el paso de las implementaciones. 

Los contendores de IoC son frameworks de DI que pueden trabajar fuera del lenguaje de programación. En algunos de ellos puedes configurar las implementaciones usando metadatos los cuales son menos invasivos.

<div class="row">
  <div class="col-md-6">
    <h4><i class="icon-file"></i> TaskManagerConstructorIoC.java</h4>
    <script type="syntaxhighlighter" class="brush: java"><![CDATA[
package com.makingdevs.ioc;

import com.makingdevs.essentials.TaskStore;

public class TaskManagerConstructorIoC {
  private TaskStore taskStore;
  private UserStore userStore;
  private LineStore lineStore;

  public TaskManagerHighTied(TaskStore taskStore,
                             UserStore userStore,
                             LineStore lineStore) {
    this.taskStore = taskStore;
    this.userStore = userStore;
    this.lineStore = lineStore;
  }

}
    ]]></script>
  </div>
  <div class="col-md-6">
    <h4><i class="icon-file"></i> TaskManagerSetterIoC.java</h4>
    <script type="syntaxhighlighter" class="brush: java"><![CDATA[
package com.makingdevs.ioc;

import com.makingdevs.essentials.TaskStore;

public class TaskManagerSetterIoC {
  private TaskStore taskStore;
  private UserStore userStore;
  private LineStore lineStore;

  public void setTaskStore(TaskStore taskStore) {
    this.taskStore = taskStore;
  }

  public void setUserStore(UserStore userStore) {
    this.userStore = userStore;
  }

  public void setLineStore(LineStore lineStore) {
    this.lineStore = lineStore;
  }
}
    ]]></script>
  </div>
</div>

Un tercer tipo de inyección de dependencias se hace a través de interfaces, las cuales permiten abstraer un nivel más los posibles elementos que podría ocupar un componente y concentrarlos en las llamadas a sus métodos indicando el tipo que necesitan.

<div class="row">
  <div class="col-md-6">
    <h4><i class="icon-file"></i> TaskComponents.java</h4>
    <script type="syntaxhighlighter" class="brush: java"><![CDATA[
package com.makingdevs.ioc;

import com.makingdevs.essentials.TaskStore;

public interface TaskComponents {
  void lineStore(LineStore lineStore);
  void userStore(UserStore userStore);
  void taskStore(TaskStore taskStore);
}
    ]]></script>
  </div>
  <div class="col-md-6">
    <h4><i class="icon-file"></i> TaskManagerInterfaceIoC.java</h4>
    <script type="syntaxhighlighter" class="brush: java"><![CDATA[
package com.makingdevs.ioc;

import com.makingdevs.essentials.TaskStore;

public class TaskManagerInterfaceIoC implements TaskComponents {
  private TaskStore taskStore;
  private UserStore userStore;
  private LineStore lineStore;

  @Override
  public void lineStore(LineStore lineStore) {
    this.lineStore = lineStore;
  }

  @Override
  public void userStore(UserStore userStore) {
    this.userStore = userStore;
  }

  @Override
  public void taskStore(TaskStore taskStore) {
    this.taskStore = taskStore;
  }
}
    ]]></script>
  </div>
</div>


<div class="bs-callout bs-callout-info">
<h4><i class="icon-coffee"></i> Información de utilidad</h4>
  <p>
  Sin lugar a dudas uno de los pensadores más influyentes es Martin Fowler, el cual, ha escrito de los <a href="http://martinfowler.com/articles/injection.html">contenedores de IoD y le patrón DI</a>, es recomendables esta lectura por que incluso hace mención del lenguaje Java y de Springframework como el ejemplo de ello.
  </p>
</div>

## Separación de las interfaces y sus implementaciones

<blockquote>
  <p>...todas las arquitecturas orientadas a objetos bien estructuradas han definido claramente sus capas, con cada capa se proporciona un conjunto coherente de servicios a través de una interfaz bien definida y controlada...</p>
  <small>Grady Booch - <cite title="Object Solutions">Object Solutions</cite></small>  
</blockquote>

### Uso de dependencias de forma directa

<div class="row">
  <div class="col-md-4">
    <h4><i class="icon-file"></i> SimpleTaskStore.java</h4>
    <script type="syntaxhighlighter" class="brush: java"><![CDATA[
package com.makingdevs.ioc;

import com.makingdevs.essentials.Task;

import javax.sql.DataSource;
import java.util.List;

public class SimpleTaskStore {

  private DataSource dataSource;

  public SimpleTaskStore() {
    // Init DataSource
  }

  public List<Task> getCurrentTasks() {
    // DB Operations are here
    new UnsupportedOperationException("Pain!!!");
  }

}
    ]]></script>
  </div>
  <div class="col-md-4">
    <h4><i class="icon-file"></i> SimpleTaskManager.java</h4>
    <script type="syntaxhighlighter" class="brush: java"><![CDATA[
package com.makingdevs.ioc;

import com.makingdevs.essentials.Task;
import java.util.List;

public class SimpleTaskManager {
  private SimpleTaskStore simpleTaskStore;

  public SimpleTaskManager(){
    this.simpleTaskStore = new SimpleTaskStore();
  }

  public List<Task> getCurrentTasks() {
    // Business Logic is here
    return simpleTaskStore.getCurrentTasks();
  }
}
    ]]></script>
  </div>
  <div class="col-md-4">
    <h4><i class="icon-file"></i> PresentationTaskManager.java</h4>
    <script type="syntaxhighlighter" class="brush: java"><![CDATA[
package com.makingdevs.ioc;

import com.makingdevs.essentials.Task;
import java.util.List;

public class PresentationTaskManager {
  private SimpleTaskManager simpleTaskManager;

  public PresentationTaskManager(){
    this.simpleTaskManager = new SimpleTaskManager();
  }

  public View showCurrentTasks(){
    List<Task> currentTasks = simpleTaskManager.getCurrentTasks();
    View view = new BlockView();
    // currentTasks + View
    return view;
  }
}
    ]]></script>
  </div>
</div>

**La recomendación para desacoplar las dependencias entre los componentes es crear abstracciones que puedan ser sustituidas por cualquier implementación.**

<div class="row">
  <div class="col-md-6">
    <h4><i class="icon-file"></i> ITaskStore.java</h4>
    <script type="syntaxhighlighter" class="brush: java"><![CDATA[
package com.makingdevs.ioc;

import com.makingdevs.essentials.Task;

import javax.sql.DataSource;
import java.util.List;

public interface ITaskStore {
  void setDataSource(DataSource dataSource);
  List<Task> getCurrentTasks();
}
    ]]></script>
  </div>
  <div class="col-md-6">
    <h4><i class="icon-file"></i> ITaskManager.java</h4>
    <script type="syntaxhighlighter" class="brush: java"><![CDATA[
package com.makingdevs.ioc;

import com.makingdevs.essentials.Task;

import java.util.List;

public interface ITaskManager {
  List<Task> getCurrentTasks();
}
    ]]></script>
  </div>
</div>

Teniendo estas abstracciones podemos usarlas en nuestros componentes agregando la dependencia a quienes desean usarlos e implementando a quienes se encargan de los detalles.

<div class="row">
  <div class="col-md-4">
    <h4><i class="icon-file"></i> TaskStoreImpl.java</h4>
    <script type="syntaxhighlighter" class="brush: java"><![CDATA[
package com.makingdevs.ioc;

import com.makingdevs.essentials.Task;

import javax.sql.DataSource;
import java.util.List;

public class TaskStoreImpl implements ITaskStore {

  private DataSource dataSource;

  public void setDataSource(DataSource dataSource){
    this.dataSource = dataSource;
  }

  public List<Task> getCurrentTasks() {
    //...
  }

}
    ]]></script>
  </div>
  <div class="col-md-4">
    <h4><i class="icon-file"></i> TaskManagerImpl.java</h4>
    <script type="syntaxhighlighter" class="brush: java"><![CDATA[
package com.makingdevs.ioc;

import com.makingdevs.essentials.Task;
import java.util.List;

public class TaskManagerImpl implements ITaskManager {

  private ITaskStore iTaskStore;

  // Constructor or setter, what do you prefer?

  public List<Task> getCurrentTasks() {
    // Business Logic is here
    // Implementation goes here
  }
}
    ]]></script>
  </div>
  <div class="col-md-4">
    <h4><i class="icon-file"></i> PresentationTaskManager.java</h4>
    <script type="syntaxhighlighter" class="brush: java"><![CDATA[
package com.makingdevs.ioc;

import com.makingdevs.essentials.Task;
import java.util.List;

public class PresentationTaskManager {

  private ITaskManager iTaskManager;

  // Constructor or setter, is your choice

  public View showCurrentTasks(){
    List<Task> currentTasks = iTaskManager.getCurrentTasks();
    // currentTasks + View
    return view;
  }
}
    ]]></script>
  </div>
</div>

La inyección de dependencias es la raíz de muchos beneficios aclamados para las tecnologías orientadas a objetos. Su aplicación adecuada es necesaria para la creación de frameworks reutilizables.Es además críticamente importante para la construcción de código que es resistente al cambio. Y, desde que las abstracciones y los detalles están aislados unos de otros, el código es mucho más fácil de mantener.