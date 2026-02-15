<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Encapsulación". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 2. Encapsulación

## 1. En Programación Orientada a Objetos (POO), ¿Qué buscan la **encapsulación** y **la ocultación** de información? Enumera brevemente algunas ventajas de la ocultación de información.

### Respuesta:

La encapsulación es el principio por el cual los datos (estado) y las operaciones que actúan sobre ellos (métodos) se agrupan dentro de una misma entidad denominada clase. La ocultación de información complementa este principio al restringir el acceso directo a los detalles internos de la representación, exponiendo únicamente una interfaz pública estable. En conjunto, ambos conceptos permiten separar el *qué* se puede hacer con un objeto del *cómo* está implementado.

Entre las ventajas de la ocultación de información se incluyen: (1) reducción del acoplamiento, puesto que otros módulos dependen solo de la interfaz y no de la implementación; (2) facilidad de mantenimiento y evolución, al poder cambiar la representación interna sin romper a los clientes; (3) control de la consistencia del estado, ya que se puede validar el acceso y las modificaciones a través de métodos; y (4) mejora de la legibilidad y del diseño, al delimitar responsabilidades claras. Frente a C estructurado, donde los `struct` suelen exponerse, la POO fomenta el acceso controlado.

---

## 2. ¿Qué se entiende por la **interfaz pública** de un objeto o clase en POO? Describe brevemente cómo se relaciona con la ocultación de información.

### Respuesta:

La interfaz pública es el conjunto de miembros accesibles desde fuera de la clase: métodos, constructores y, en su caso, constantes públicas. Esta interfaz define el contrato observable por los clientes: qué operaciones se ofrecen, con qué parámetros y qué efectos o resultados producen. Es, por tanto, la “cara externa” de la clase.

Su relación con la ocultación de información es directa: al exponer solo lo necesario, se mantiene privada la representación y la lógica interna no destinada al exterior. De esta forma, la clase puede modificar estructuras internas, algoritmos o campos privados sin impacto en el código cliente, siempre que la interfaz pública y sus garantías se conserven.

---

## 3. Brevemente: ¿Por qué hay que ser conscientes y diseñar con cuidado la **interfaz pública** de una clase? ¿Es fácil cambiarla?

### Respuesta:

La interfaz pública es un contrato que otros módulos, librerías o programas consumen. Un diseño descuidado introduce dependencias innecesarias, operaciones mal definidas o ambiguas, y puntos de extensión erróneos que luego complican la evolución. Al tratarse del punto de contacto, conviene que sea mínima, coherente, expresiva y orientada al dominio.

Cambiarla no suele ser fácil porque implica modificaciones en todo el código que la usa: renombrar métodos, cambiar tipos de parámetros o contratos comporta refactorizaciones amplias y, potencialmente, problemas de compatibilidad. Por el contrario, cambiar detalles internos privados es mucho menos costoso y más seguro.

---

## 4. ¿Qué son las **invariantes de clase** y por qué la ocultación de información nos ayuda?

### Respuesta:

Una invariante de clase es una condición lógica que debe cumplirse para cualquier instancia válida de la clase en todos los momentos observables, excepto durante operaciones internas en las que dicha condición puede estar temporalmente suspendida (por ejemplo, dentro de un método modificador hasta su finalización). Ejemplos típicos son “el radio debe ser no negativo” o “la suma de probabilidades debe ser 1”.

La ocultación de información ayuda a preservar estas invariantes porque centraliza los puntos donde se modifica el estado. Al impedir la escritura directa de campos, se obliga a pasar por métodos que pueden validar, normalizar o rechazar datos, manteniendo la coherencia global. Esto resulta esencial para evitar estados inválidos difíciles de depurar.

---

## 5. Pon un ejemplo de una clase `Punto` en `Java`, con dos coordenadas, `x` e `y`, de tipo `double`, con un método `calcularDistanciaAOrigen`, y que haga uso de la ocultación de información. ¿Cuál es la interfaz pública de la clase `Punto`? ¿Qué significa `public` y `private`?

### Respuesta:

En este diseño, la ocultación de información se logra declarando los campos `x` e `y` como `private` y exponiendo solo lo necesario mediante `public`. La interfaz pública incluirá el constructor, los getters (si se consideran necesarios) y los métodos de consulta como `calcularDistanciaAOrigen`. Con ello, se evita el acceso directo a la representación interna y se mantiene control sobre su validez.

En Java, `public` indica que el miembro es accesible desde cualquier clase; `private` restringe el acceso al propio cuerpo de la clase. Esto permite que la implementación interna pueda cambiar (por ejemplo, almacenando coordenadas en otro formato) sin obligar a los clientes a modificar su uso mientras se conserve la interfaz pública.

Código:
```java
public class Punto {
    // Representación oculta (ocultación de información)
    private double x;
    private double y;

    // Interfaz pública (parte del contrato)
    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double getX() { return x; }
    public double getY() { return y; }

    public double calcularDistanciaAOrigen() {
        return Math.hypot(x, y); // equivalente a sqrt(x^2 + y^2) con buena estabilidad numérica
    }
}
```

---

## 6. En Java, ¿A quiénes se pueden aplicar los modificadores `public` o `private`?

### Respuesta:

En Java, `public` y `private` pueden aplicarse a miembros de clase (atributos, métodos, constructores) y, con matices, a tipos. A nivel de tipos superiores (top-level), las clases, interfaces y enums pueden ser `public` o con visibilidad por defecto (package-private). No es posible declarar una clase top-level como `private`.

Dentro de una clase (tipos anidados), se permite `public`, `private`, `protected` y por defecto (package-private) para clases, interfaces, enums y clases anidadas estáticas o internas. En cuanto a miembros, los campos, métodos y constructores admiten `public`, `private`, `protected` o visibilidad por defecto.

---

## 7. En POO, la visibilidad puede ser pública o privada, pero ¿existen más tipos de visibilidad? ¿Qué ocurre en Java? ¿Y en otros lenguajes?

### Respuesta:

Además de `public` y `private`, muchos lenguajes incorporan visibilidades intermedias para equilibrar encapsulación y reutilización. En Java existen `protected` (accesible en la clase, subclases y el mismo paquete) y la visibilidad por defecto, conocida como package-private (accesible solo dentro del mismo paquete).

Otros lenguajes ofrecen conjuntos distintos: C++ tiene `public`, `protected` y `private` sin noción de paquete; C# añade `internal`, `protected internal` y `private protected` para granularidad de ensamblado; Kotlin incorpora `internal` para el módulo. Python se basa en convenciones y *name mangling* (doble guion bajo) en lugar de visibilidades estrictas del compilador.

---

## 8. Responde: Los miembros de instancia privados de un objeto están ocultos para (a) otras clases o (b) otras instancias, aunque sean de la misma clase. Pon un ejemplo añadiendo un método `calcularDistanciaAPunto(Punto otro)` y explica la respuesta.

### Respuesta:

Los miembros privados están ocultos para **otras clases**, pero **no** para **otras instancias de la misma clase** cuando se accede desde el propio código de la clase. Es decir, dentro de un método de `Punto`, se puede leer el `x` y `y` de *otro* `Punto` aunque sean `private`, porque el acceso ocurre dentro del contexto de la clase `Punto`.

Este detalle es clave para operaciones como comparaciones, copias o cálculos entre instancias homogéneas. Permite mantener la representación verdaderamente privada frente a terceros, sin obstaculizar la cooperación entre objetos del mismo tipo a nivel de implementación.

Código:
```java
public double calcularDistanciaAPunto(Punto otro) {
    // Acceso legal a campos privados de "otro" porque estamos dentro de la clase Punto
    return Math.hypot(this.x - otro.x, this.y - otro.y);
}
```

---

## 9. ¿Qué son los métodos "getter" y "setter" en los lenguajes orientados a objetos?

### Respuesta:

Los *getters* (accesores) son métodos de lectura que exponen de forma controlada el valor de un atributo. Los *setters* (mutadores) son métodos de escritura que permiten modificar un atributo, normalmente aplicando validaciones, normalizaciones o disparando efectos colaterales controlados.

Su uso debe ser intencional: exponer un getter o setter no es automático ni obligatorio. Abrir la escritura indiscriminada con setters puede poner en riesgo invariantes y aumentar el acoplamiento. Por ello, con frecuencia se prefieren métodos de dominio que expresen operaciones significativas antes que setters genéricos.

---

## 10. Cuando nos referimos a que la ocultación de información mejora la "seguridad" del programa, ¿nos referimos a que no pueda ser "hackeado"?

### Respuesta:

En este contexto, “seguridad” se refiere principalmente a la **integridad** y **robustez** internas del diseño: evitar estados inválidos, reducir efectos colaterales, impedir usos indebidos de la representación y acotar el acoplamiento. La encapsulación dificulta alteraciones accidentales o no controladas del estado.

No debe confundirse con la seguridad en el sentido de ciberseguridad (resistencia a ataques externos). Aunque reducir la superficie pública puede aportar beneficios indirectos (menos vectores de interacción), la protección contra ataques requiere medidas adicionales: validación de entradas, control de acceso, criptografía, políticas de despliegue, etc.

---

## 11. ¿Qué diferencia hay entre **miembro de instancia** y **miembro de clase**? ¿Los miembros de clase también se pueden ocultar?

### Respuesta:
Un miembro de instancia pertenece a cada objeto concreto (cada instancia mantiene su propia copia o estado), mientras que un miembro de clase (en Java, marcado con `static`) pertenece a la clase en sí, compartiéndose entre todas las instancias. Por ejemplo, un contador total de objetos suele ser estático, mientras que las coordenadas de un `Punto` son de instancia.

Los miembros de clase también pueden y deben encapsularse: pueden ser `private static` y exponerse mediante métodos públicos controlados (también estáticos, si procede). El hecho de ser compartidos no elimina la necesidad de ocultación de información ni las validaciones correspondientes.

---

## 12. Brevemente: ¿Tiene sentido que los constructores sean privados?

### Respuesta:

Sí, tiene sentido cuando se desea **controlar** por completo cómo se crean las instancias. Un constructor `private` impide la instanciación directa desde fuera de la clase, forzando a utilizar métodos factoría estáticos (p. ej., `of(...)`, `from(...)`), *builders* o patrones como *singleton*.

Este enfoque permite validar, aplicar cachés o *pools*, normalizar parámetros, devolver subtipos o instancias compartidas, e incluso impedir la creación de objetos si no se cumplen ciertas precondiciones. También es útil en clases de utilidades que no deben instanciarse nunca.

---

## 13. ¿Cómo se indican los **miembros de clase** en Java? Pon un ejemplo, en la clase `Punto` definida anteriormente, para que incluya miembros de clase que permitan saber cuáles son los valores `x` e `y` máximos que se han establecido en todos los puntos que se hayan creado hasta el momento.

### Respuesta:

En Java, los miembros de clase se indican con la palabra clave `static`. Pueden declararse `private` para ocultarlos y exponer solo lecturas mediante getters estáticos `public` si se desea consultar su valor externo. Se recomienda inicializarlos con valores neutros y actualizarlos en los puntos de creación o modificación pertinentes.

A continuación, se muestra una versión de `Punto` que mantiene los máximos globales de `x` e `y` observados en todas las instancias. Se encapsulan los campos estáticos y se actualizan en el constructor. La interfaz pública expone métodos de lectura estáticos para consultar estos máximos.

 Código:
```java
public class Punto {
    private double x;
    private double y;

    // Miembros de clase (estáticos) ocultos
    private static double maxX = Double.NEGATIVE_INFINITY;
    private static double maxY = Double.NEGATIVE_INFINITY;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
        // Actualización controlada de los máximos globales
        if (x > maxX) maxX = x;
        if (y > maxY) maxY = y;
    }

    public double getX() { return x; }
    public double getY() { return y; }

    public double calcularDistanciaAOrigen() {
        return Math.hypot(x, y);
    }

    public double calcularDistanciaAPunto(Punto otro) {
        return Math.hypot(this.x - otro.x, this.y - otro.y);
    }

    // Interfaz pública estática para consultar máximos
    public static double getMaxXGlobal() { return maxX; }
    public static double getMaxYGlobal() { return maxY; }
}
```

---

## 14. Como sería un método factoría dentro de la clase `Punto` para construir un `Punto` a partir de dos coordenadas, pero que las redondee al entero más cercano. Escribe sólo el código del método, no toda la clase ¿Has usado `static`? 

### Respuesta:

El método factoría se declara `static` para asociarlo a la clase y no a una instancia concreta. De este modo, se puede crear un `Punto` sin requerir un objeto previo y se puede centralizar la lógica de normalización o validación. Para redondear al entero más cercano, puede emplearse `Math.rint` (que devuelve `double`) o `Math.round` (que devuelve `long`), convirtiendo a `double` si se desea mantener el tipo.

Se utiliza `static` porque define una operación de construcción propia de la clase como tipo, no de una instancia ya creada. Esto ayuda a mejorar la legibilidad (`Punto.redondeado(...)`) y a evitar constructores con banderas ambiguas.

Solo el método (para incluir dentro de Punto):
```java
public static Punto crearRedondeado(double x, double y) {
    double rx = Math.rint(x); // redondeo al entero más cercano como double (ties-to-even)
    double ry = Math.rint(y);
    return new Punto(rx, ry);
}```

---

## 15. Cambia la implementación de `Punto`. En vez de dos `double`, emplea un array interno de dos posiciones, intentando no modificar la interfaz pública de la clase.

### Respuesta:

Para mantener la interfaz pública, se conservan los mismos nombres y firmas de métodos (constructor, getters, etc.), pero la representación privada pasa a ser un arreglo `double[2]`. Este cambio de implementación no debería afectar a los clientes mientras se respete el contrato expuesto.

Esto ilustra el beneficio de la ocultación de información: la representación interna puede variar libremente. Incluso se pueden agregar validaciones y normalizaciones internas adicionales sin alterar el código cliente que usa la clase `Punto`.

Código:
```java
public class Punto {
    // Representación interna cambiada: 0 -> x, 1 -> y
    private final double[] coord = new double[2];

    public Punto(double x, double y) {
        coord[0] = x;
        coord[1] = y;
    }

    public double getX() { return coord[0]; }
    public double getY() { return coord[1]; }

    public double calcularDistanciaAOrigen() {
        return Math.hypot(coord[0], coord[1]);
    }

    public double calcularDistanciaAPunto(Punto otro) {
        // Acceso a representación privada de "otro" permitido dentro de la clase
        return Math.hypot(this.coord[0] - otro.coord[0],
                          this.coord[1] - otro.coord[1]);
    }
}
```

---

## 16. Si un atributo va a tener un método "getter" y "setter" públicos, ¿no es mejor declararlo público? ¿Cuál es la convención más habitual sobre los atributos, que sean públicos o privados? ¿Tiene esto algo que ver con las "invariantes de clase"?

### Respuesta


## 17. ¿Qué significa que una clase sea **inmutable**? ¿qué es un método modificador? ¿Un método modificador es siempre un "setter"? ¿Tiene ventajas que una clase sea inmutable?

### Respuesta


## 18. ¿Es recomendable incluir métodos "setter" siempre y como convención?

### Respuesta


## 19. ¿La clase `String` en Java es mutable o inmutable? ¿Qué ocurre al concatenar dos cadenas? ¿Qué debemos hacer si vamos a hacer una operación que implique concatenar muchas veces para construir paso a paso una cadena muy larga?

### Respuesta


## 20. En POO ¿Cómo se comparan objetos de una misma clase? ¿Por su contenido o por su identidad? ¿Qué es el método equals en Java? ¿Qué hace por defecto? ¿Cómo se deben comparar dos cadenas en Java? 

### Respuesta


## 21. ¿Qué son las clases "wrapper" en un lenguaje de programación orientado a objetos? ¿Cómo se hace? ¿Es un proceso automático? ¿Qué ventajas tienen? ¿Todos los lenguajes orientados a objetos tienen tipos primitivos y necesitan wrappers? 

### Respuesta


## 22. ¿En POO qué es un **tipo de dato enumerado**? ¿En Java, un tipo de dato enumerado es una clase? ¿Qué ventajas tienen en términos de encapsulación los enumerados en Java?

### Respuesta


## 23. Crea un tipo enumerado en Java que se llame `Mes`, con doce posibles instancias y que además proporcione métodos para obtener cuántos días tiene ese mes, el ordinal de ese mes en el año (1-12), empleando atributos privados y constructores del tipo enumerado.

### Respuesta


## 24. Añade a la clase `Mes` del ejercicio anterior cuatro métodos para devolver si ese mes tiene algunos días de invierno, primavera, verano u otoño, indicando con un booleano el hemisferio (norte o sur, parámetro `enHemisferioNorte`). Es decir: `esDePrimavera(boolean esHemisferioNorte)`, `esDeVerano(boolean esHemisferioNorte)`, `esDeOtoño(boolean esHemisferioNorte)`, `esDeInvierno(boolean esHemisferioNorte)`

### Respuesta
