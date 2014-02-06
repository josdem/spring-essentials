# Uso de SpEL y Profiles

------

## Spring Expression Language

El lenguaje de Expresión de Spring es un poderoso lenguaje de expresión ue soporta la búsqueda y manipulación de un objeto como grafo en tiempo de ejecución. El cual, fue creado para ser usado en varios subproyectos de Spring.

No esta directamente atado a Spring, por lo que puede ser usado de forma independiente(es decir, no necesita un `ApplicationContext`). Aunque mayormente, estremos usando el SpEL dentro del contenedor de Spring.

Le lenguaje de expresión soporta la siguiente funcionalidad:

* Expresion de literales
* Operadores relacionales y booleanos
* Expresiones regulares
* Expresiones de clases
* Acceder a propiedades, arreglos, listas y mapas
* Invocación de métodos
* Asignaciones
* Llamadas a constructores
* Referencias a beans
* Construcción de arreglos
* Listas en línea
* Operador ternario
* Variables
* Funciones definidas por el usuario
* Proyección de colecciones
* Selección de colecciones
* Expresiones en plantillas

La interface `ExpressionParser` es responsable del parseo de las expresiones, puede arrojar dos excepciones: `ParseException` y `EvaluationException`; y los paquetes de uso principales para el manejo de SpEL son: `org.springframework.expression` y el subpaquete `spel.support`.

<div class="row">
  <div class="col-md-12">
    <h4><i class="icon-file"></i> SpELInterfaceTests.java</h4>
    <script type="syntaxhighlighter" class="brush: java"><![CDATA[
package com.makingdevs.practica13;

import static org.springframework.util.Assert.isTrue;

import java.util.Date;
import java.util.GregorianCalendar;

import org.junit.Test;
import org.springframework.expression.EvaluationContext;
import org.springframework.expression.Expression;
import org.springframework.expression.ExpressionParser;
import org.springframework.expression.spel.standard.SpelExpressionParser;
import org.springframework.expression.spel.support.StandardEvaluationContext;

public class SpELInterfaceTests {

  ExpressionParser parser = new SpelExpressionParser();

  @Test
  public void testStringExpression() {
    Expression exp = parser.parseExpression("'Making Devs'");
    String message = (String) exp.getValue();
    isTrue("Making Devs".equals(message));
  }

  @Test
  public void testMethodInvocation() {
    Expression exp = parser.parseExpression("('We are ' + 'Making Devs').concat('!!!')");
    String message = (String) exp.getValue();
    isTrue("We are Making Devs!!!".equals(message));
  }

  @Test
  public void testTypeInvocation() {
    Expression exp = parser.parseExpression("'Making Devs'.bytes");
    byte[] bytes = (byte[]) exp.getValue();
    isTrue(new String(bytes).equals("Making Devs"));
    exp = parser.parseExpression("'We are making software'.bytes.length");
    int length = (Integer) exp.getValue();
    isTrue("We are making software".getBytes().length == length);
  }

  @Test
  public void testValueByType() {
    ExpressionParser parser = new SpelExpressionParser();
    Expression exp = parser.parseExpression("new String('Software development').toUpperCase()");
    String message = exp.getValue(String.class);
    isTrue("SOFTWARE DEVELOPMENT".equals(message));
  }
  
  @Test
  public void testGetValueFromADifferentContext() {
    GregorianCalendar calendar = new GregorianCalendar(2013, 6, 12);
    ExpressionParser parser = new SpelExpressionParser();
    Expression exp = parser.parseExpression("time");
    EvaluationContext context = new StandardEvaluationContext(calendar);
    Date date = (Date) exp.getValue(context);
    isTrue(date.equals(calendar.getTime()));
  }

}
    ]]></script>
  </div>
</div>

En el uso independiente de SpEL existe la necesidad de crear un parser, parsear las expresiones y quiza proveer de la evaluación de contextos de evaluación y objeto raíz de contexto. Sin embargo, el uso más común es en archivos de configuración como parte de la asignación de los valores establecidos a los beans.

<div class="row">
  <div class="col-md-12">
    <h4><i class="icon-file"></i> UsingLanguageTests.java</h4>
    <script type="syntaxhighlighter" class="brush: java"><![CDATA[
package com.makingdevs.practica13;

import org.junit.Test;
import org.springframework.expression.Expression;
import org.springframework.expression.ExpressionParser;
import org.springframework.expression.spel.standard.SpelExpressionParser;
import org.springframework.util.Assert;
import static org.springframework.util.Assert.*;
import static org.junit.Assert.*;

public class UsingLanguageTests {

  ExpressionParser parser = new SpelExpressionParser();

  @Test
  public void testInvokeStaticMethod() {
    // El prefijo 'T' indica el tipo, en este caso la clase Math
    Expression exp = parser.parseExpression("T(Math).random() * 100.0");
    // En la expresion anterior, el resultado de la llamada a random
    double value = exp.getValue(double.class);
    Assert.notNull(value);
    System.out.println(value);
  }

  @Test
  public void testRelationalOperators() {
    ExpressionParser parser = new SpelExpressionParser();
    isTrue(parser.parseExpression("2==2").getValue(boolean.class));
    isTrue(parser.parseExpression("2<3").getValue(boolean.class));
    isTrue(parser.parseExpression("3>2").getValue(boolean.class));
    isTrue(parser.parseExpression("0!=1").getValue(boolean.class));
  }

  @Test
  public void testLogicalOperators() {
    isTrue(parser.parseExpression("true and true").getValue(boolean.class));
    isTrue(parser.parseExpression("true or true").getValue(boolean.class));
    isTrue(parser.parseExpression("!false").getValue(boolean.class));
    isTrue(parser.parseExpression("not false").getValue(boolean.class));
    isTrue(parser.parseExpression("true and not false").getValue(boolean.class));
  }

  @Test
  public void testMathematicalOperators() {
    ExpressionParser parser = new SpelExpressionParser();
    assertSame(2, parser.parseExpression("1+1").getValue(int.class));
    assertSame(0, parser.parseExpression("1-1").getValue(int.class));
    assertSame(1, parser.parseExpression("1/1").getValue(int.class));
    assertSame(1, parser.parseExpression("1*1").getValue(int.class));
    assertSame(1, parser.parseExpression("1^1").getValue(int.class));
    assertTrue(1D == parser.parseExpression("1e0").getValue(double.class));
    assertEquals("foobar", parser.parseExpression("'foo'+'bar'").getValue(String.class));
  }

  @Test
  public void testTernaryElvisAndSafeNavigationOperators() {
    assertEquals("foo", parser.parseExpression("true ? 'foo' : 'bar'").getValue(String.class));
    assertEquals("es nulo", parser.parseExpression("null?:'es nulo'").getValue(String.class));
    assertEquals(null, parser.parseExpression("null?.foo").getValue(String.class));
  }

}
    ]]></script>
  </div>
</div>

### Uso del SpEL dentro de los archivos de configuración

## Spring Profiles

