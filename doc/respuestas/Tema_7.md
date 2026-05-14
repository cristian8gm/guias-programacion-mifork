<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Aspectos funcionales". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: clases y objetos, encapsulación, excepciones, composición, herencia, polimorfismo y genericidad.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->

# TEMA 7. Aspectos funcionales

## 1. ¿Qué es un puntero a una función? Pon un ejemplo de código en C, donde se define una función y que reciba una cadena de caracteres como parámetro y devuelva la cadena en mayúsculas. Crea un puntero en una variable local a dicha función llamado `aMayusculas` e invócala con el puntero.

### Respuesta:

Un puntero a función en C es una variable que almacena la dirección de memoria de una función. Esto permite tratar a las funciones como datos: se pueden asignar a variables, pasar como parámetros o devolver como resultado. Es una técnica fundamental en C para implementar comportamientos flexibles, como callbacks o estrategias intercambiables.

El uso de punteros a función no implica cierre de contexto ni captura de variables externas. La función apuntada solo puede operar con los parámetros que se le pasan explícitamente, lo que limita su expresividad frente a mecanismos funcionales más modernos.

Código C:
```C
#include <ctype.h>
#include <stdio.h>

void aMayusculas(char* s) {
    for (int i = 0; s[i] != '\0'; i++) {
        s[i] = toupper(s[i]);
    }
}

int main() {
    void (*aMayusculasPtr)(char*) = aMayusculas;

    char texto[] = "hola mundo";
    aMayusculasPtr(texto);
    printf("%s\n", texto);
}
```

---

## 2. ¿Qué es una **función lambda** en un lenguaje de programación? Pon un ejemplo similar al anterior en Javascript y otro en Java con funciones lambda. Usa una variable local `aMayusculas` para apuntar a la función lambda. Por simplicidad, en Java, emplea `Function<String, String>` para el tipo de la referencia a la función lambda.

### Respuesta:

Una función lambda es una función anónima que puede definirse de forma concisa y asignarse a una variable o pasarse como argumento. Suelen emplearse para expresar comportamientos simples sin necesidad de definir funciones con nombre. Son especialmente útiles cuando se usan como parámetros de otras funciones.

A diferencia de los punteros a función en C, las funciones lambda pueden capturar variables del entorno donde se definen, formando cierres (*closures*). Esto aumenta notablemente su expresividad y potencia.

JavaScript:
```Java
let aMayusculas = s => s.toUpperCase();
console.log(aMayusculas("hola"));
```

Java:
```Java
import java.util.function.Function;

Function<String, String> aMayusculas = s -> s.toUpperCase();
System.out.println(aMayusculas.apply("hola"));
```

---

## 3. ¿Qué es el **paradigma funcional**? ¿Por qué a algunos lenguajes orientados a objetos como Java 8, se les llama multi-paradigma? ¿Qué quiere decir que las funciones son "ciudadanos de primera clase"?

### Respuesta:

El paradigma funcional es un estilo de programación que trata las funciones como elementos centrales del diseño. Se enfatiza el uso de funciones puras, la ausencia de efectos secundarios y el uso de funciones como valores que pueden pasarse y devolverse.

Lenguajes como Java se consideran multi-paradigma porque, aunque nacieron como orientados a objetos, incorporan desde Java 8 características funcionales. Decir que las funciones son “ciudadanos de primera clase” implica que pueden almacenarse en variables, pasarse como argumentos y devolverse como resultados, al igual que cualquier otro dato.

---

## 4. Explica la sintaxis básica de una función lambda en Java.

### Respuesta:

La sintaxis básica de una función lambda en Java consta de tres partes: parámetros, operador `->` y cuerpo. Los parámetros pueden ir entre paréntesis, el cuerpo puede ser una expresión o un bloque de código. El tipo de la lambda no se indica explícitamente, sino que se infiere del contexto.

Esta inferencia se basa en la interfaz funcional a la que se asigna la lambda. Por ello, una lambda en Java siempre tiene un tipo bien definido, aunque no se escriba de forma explícita.

Ejemplo:
(x, y) -> x + y

---

## 5. Ahora recibamos una función como parámetro a un método y la llamaremos desde dentro. Amplia los ejemplos anteriores de Java y JavaScript con un método llamado `transformar`, que reciba un `String` como parámetro y luego una función transformadora como lo es `aMayúsculas` y la invoque desde dentro.

### Respuesta:

Una de las características clave del enfoque funcional es la posibilidad de pasar funciones como argumentos. Esto permite parametrizar el comportamiento de un método sin necesidad de herencia ni condicionales complejos.

En el ejemplo, el método `transformar` recibe una cadena y una función que define cómo transformar esa cadena. El método no conoce el detalle de la transformación, solo se limita a invocar la función recibida.

Java:
```Java
static String transformar(String s, Function<String, String> f) {
    return f.apply(s);
}
```

JavaScript:
```Java
function transformar(s, f) {
    return f(s);
}
```

---

## 6. Ahora, invoca `transformar`, con una nueva función lambda directamente en la llamada a `transformar`, por ejemplo, una función lambda que invierta la cadena. Define la función de inversión justo cuando la estás pasando como parámetro.

### Respuesta:

Las funciones lambda pueden definirse directamente en el punto donde se usan, sin necesidad de asignarlas previamente a una variable. Esto es habitual cuando la función solo se emplea una vez y su comportamiento es sencillo.

Este estilo favorece un código más expresivo y compacto, donde el comportamiento se describe justo en el lugar donde se necesita.

Java:
```Java
String r = transformar("hola", s -> new StringBuilder(s).reverse().toString());
```

JavaScript:
```Java
let r = transformar("hola", s => s.split("").reverse().join(""));
```

---

## 7. ¿Qué se entiende por cierre o "closure" en el contexto de las funciones lambda? Pon un ejemplo en Java de cómo una función lambda es capaz de acceder a una variable local en el contexto donde fue definida. Modifica el ejemplo anterior, creando otra función lambda para transformar una cadena, pero que lo que haga es concatenar a la cadena de entrada otra cadena que está en una variable local definida fuera de la función lambda.

### Respuesta:

Un cierre ocurre cuando una función lambda captura variables del entorno donde fue definida. Estas variables no forman parte de los parámetros de la función, pero siguen siendo accesibles mientras la función exista.

En Java, las variables capturadas deben ser efectivamente finales. Esto garantiza coherencia y evita efectos secundarios difíciles de razonar. El siguiente ejemplo muestra cómo una lambda usa una variable externa.

Java:
```Java
String sufijo = "!!!";
Function<String, String> transformar = s -> s + sufijo;
System.out.println(transformar.apply("hola"));
```

---

## 8. Reflexiona: ¿en qué se diferencia entonces una función lambda de los punteros a funciones que hay en C?

### Respuesta:

Un puntero a función en C solo referencia una función concreta y no captura contexto alguno. Toda la información necesaria debe pasarse explícitamente mediante parámetros. Esto limita su capacidad para expresar comportamientos dependientes del entorno.

Las funciones lambda, en cambio, pueden capturar variables y formar cierres, lo que permite construir funciones “configuradas” dinámicamente. Esta diferencia es clave para entender la mayor expresividad del enfoque funcional moderno.

---

## 9. Devolvamos ahora funciones. Creemos ahora una función que sea capaz de crear funciones "descuento". Una función "descuento", decrementa un porcentaje pasado como parámetro. Por simplicidad, usa `Function<Double, Double>` para su tipo. La función `crearDescuento(porcentaje)`, recibe solo el porcentaje de descuento a aplicar y devuelve la función de descuento. Prueba a crear dos descuentos distintos y aplicarlos a una cantidad. Explica la closure en la función descuento.

### Respuesta:

Una función puede devolver otra función, creando comportamientos especializados a partir de parámetros iniciales. En este caso, se crea una función que aplica un descuento concreto, capturando el porcentaje en un cierre.

El porcentaje queda almacenado en el entorno de la lambda devuelta. Cada función descuento mantiene su propio valor capturado, demostrando claramente el concepto de closure.

Java:
```Java
static Function<Double, Double> crearDescuento(double porcentaje) {
    return precio -> precio * (1 - porcentaje);
}

Function<Double, Double> d10 = crearDescuento(0.10);
Function<Double, Double> d20 = crearDescuento(0.20);

System.out.println(d10.apply(100.0));
System.out.println(d20.apply(100.0));
```

---

## 10. En Java, que es un lenguaje con comprobación estática de tipos, donde los tipos se declaran, toda función lambda tiene un tipo, que se conoce como **interfaz funcional**. ¿Qué es una **interfaz funcional**? ¿Qué requisitos tiene?

### Respuesta:

En Java, una interfaz funcional es una interfaz que define exactamente un método abstracto. Es el tipo al que se asocian las funciones lambda, permitiendo que el compilador verifique la compatibilidad de tipos.

Puede contener métodos por defecto o estáticos, pero solo un método abstracto. Este requisito permite que el compilador infiera de forma inequívoca qué método implementa la lambda.

---

## 11. Creemos una interfaz funcional a mano. Por ejemplo, define la interfaz funcional del ejemplo que transforma la cadena en otra. Llámale `Transformador`, que define una función que convierte una cadena de texto (`String`) en otra (`String`).

### Respuesta:

Se puede definir una interfaz funcional personalizada cuando se desea expresar con claridad la intención del código. En este caso, `Transformador` modela una transformación de una cadena en otra.

Definir interfaces funcionales propias mejora la legibilidad frente al uso directo de interfaces genéricas cuando el dominio lo justifica.

Código:
```Java
@FunctionalInterface
interface Transformador {
    String transformar(String s);
}
```

---

## 12. Ahora hagamos la interfaz funcional algo más genérica y empleando generics, para que permita definir un `Transformador` de un tipo en otro. Pon un ejemplo de un transformador que redondea un `Double` en un `Integer`.

### Respuesta:

Al añadir generics, la interfaz funcional se vuelve reutilizable para múltiples tipos. Esto permite definir transformaciones entre distintos tipos sin perder seguridad en el chequeo.

El ejemplo muestra un transformador que convierte un `Double` en un `Integer`, aprovechando la genericidad.

Código:
```Java
@FunctionalInterface
interface Transformador<T, R> {
    R transformar(T valor);
}

Transformador<Double, Integer> redondear = d -> (int) Math.round(d);
```

---

## 13. `Transformador`, en su versión genérica, parece muy útil y reutilizable, hasta el punto de que es igual a una interfaz funcional que ya hay, que es `Function<T, R>`. Muestra las interfaces funcionales predefinidas que hay en Java.

### Respuesta:

Java proporciona un conjunto amplio de interfaces funcionales estándar en el paquete `java.util.function`. Estas cubren los casos más habituales y evitan la necesidad de definir interfaces propias.

Algunas de las más usadas son: `Function<T, R>`, `Consumer<T>`, `Supplier<T>`, `Predicate<T>`, `UnaryOperator<T>` y `BinaryOperator<T>`. Todas siguen el modelo de una única operación abstracta.

---

## 14. Vamos a ver ejemplos expresivos de funcional en Java. Estudiemos el `List.forEach`, como versión funcional del bucle `for`. Emplea el `forEach` para recorrer una lista de `Integer` y que muestre un mensaje si el entero es positivo.

### Respuesta:

El método `forEach` permite recorrer colecciones de forma funcional, pasando una acción a ejecutar sobre cada elemento. Esto elimina la necesidad de bucles explícitos y favorece un estilo declarativo.

El ejemplo muestra cómo imprimir un mensaje solo para los enteros positivos de una lista, usando una lambda.

Código:
```Java
List<Integer> nums = List.of(-2, 3, 0, 5);
nums.forEach(n -> {
    if (n > 0) System.out.println("Positivo: " + n);
});
```

---


## 15. Repasando el tema de genericidad, fíjate en la firma de `forEach`, ¿por qué se usa `Consumer<? super T>` y no `Consumer<T>`? Explica qué significa **PECS**, y explícalo para el caso de mejorar el ejemplo del método `transformar` la hora de definir el tipo de la función transformadora.

### Respuesta:

PECS significa *Producer Extends, Consumer Super*. Indica que si una estructura produce valores, se debe usar `extends`, y si consume valores, se debe usar `super`.

`forEach` consume elementos de la colección, por lo que acepta un `Consumer<? super T>`. Esto aumenta la flexibilidad del método. El mismo principio puede aplicarse al método `transformar` para aceptar funciones más generales sin perder seguridad.

---

## 16. Referencias a métodos. Podemos obtener una referencia a métodos de objetos o clases. Pon un ejemplo en JavaScript y en Java, de una clase `Persona` con un método `saludar`. En el código principal, crea una `Persona` con un nombre, y obtén una referencia a su método `saludar` en una variable local. Invoca `saludar` con esa referencia a su método `saludar`.

### Respuesta:

Las referencias a métodos permiten tratar métodos existentes como funciones, sin necesidad de escribir una lambda explícita. Son una forma más concisa cuando la lambda solo delega en un método.

Se pueden usar tanto en JavaScript como en Java, y mantienen el enlace con el objeto cuando se trata de métodos de instancia.

JavaScript:
```Java
class Persona {
    constructor(nombre) { this.nombre = nombre; }
    saludar() { console.log("Hola, soy " + this.nombre); }
}
let p = new Persona("Ana");
let ref = p.saludar.bind(p);
ref();
```

Java:
```Java
class Persona {
    private String nombre;
    public Persona(String nombre) { this.nombre = nombre; }
    public void saludar() {
        System.out.println("Hola, soy " + nombre);
    }
}

Persona p = new Persona("Ana");
Runnable r = p::saludar;
r.run();
```

---

## 17. ¿Qué tipos de referencias a método se pueden hacer en Java? Pon un ejemplo de referencia a método estático, a constructor, a método de instancia de una instancia concreta y a método de instancia sobre cualquier instancia.

### Respuesta:

Java permite cuatro tipos principales de referencias a método. Cada una cubre un patrón habitual de uso y sustituye a lambdas simples.

Ejemplos:
```Java
Clase::metodoEstatico
Persona::new
p::saludar
Persona::saludar
```

---

## 18. Otro ejemplo expresivo. Ordena una lista de `Persona`, cada persona tiene un nombre y una edad (de tipo entero). Ordena la lista de `Persona` con `Collections.sort`, pasándole como comparador una expresión lambda que compare la edad de ambas personas y si tienen la misma edad, se ordene por orden alfabético del nombre. Crea dos versiones: Una con la función de comparación hecha manualmente, y otra empleando `Comparator`.

### Respuesta:

Las expresiones lambda permiten definir comparadores de forma clara y concisa. Esto evita clases anónimas verbosas y hace que la lógica de ordenación sea más legible.

Se muestra una versión manual y otra usando utilidades de `Comparator`, que resultan más expresivas y reutilizables.

Código:
```Java
class Persona {
    String nombre;
    int edad;
}

Collections.sort(personas, (p1, p2) -> {
    if (p1.edad != p2.edad)
        return p1.edad - p2.edad;
    return p1.nombre.compareTo(p2.nombre);
});

Collections.sort(personas,
    Comparator.comparingInt((Persona p) -> p.edad)
              .thenComparing(p -> p.nombre)
);
```