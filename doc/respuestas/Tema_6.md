<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Genericidad". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: clases y objetos, encapsulación, excepciones, composición, herencia y polimorfismo.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 6. Genericidad

## 1. Empleando `void*` en C o `Object` en Java, pon un ejemplo de una estructura de datos, que empleando un array primitivo, permita alojar cualquier tipo de dato.

### Respuesta:

En C, se puede usar `void*` para construir una estructura capaz de almacenar cualquier tipo de dato. Esto permite flexibilidad, pero obliga a realizar conversiones manuales (*casting*) al recuperar los valores. No existe chequeo de tipos en tiempo de compilación, lo que introduce posibles errores difíciles de detectar.

En Java, se puede utilizar `Object` como tipo base universal. Dado que todas las clases heredan de `Object`, una estructura que almacene `Object` puede contener cualquier tipo, pero también obliga a hacer *downcasting* manual al recuperar los valores.

Código C:
```C
typedef struct {
    void* datos[10];
} Lista;
```

Código Java:
```Java
class Lista {
    private Object[] datos = new Object[10];

    public void set(int i, Object valor) {
        datos[i] = valor;
    }

    public Object get(int i) {
        return datos[i];
    }
}
```

---

## 2. Brevemente, ¿Qué significa la **programación genérica**? ¿Es el ejemplo anterior un ejemplo básico de programación genérica? 

### Respuesta:

La programación genérica consiste en definir estructuras, clases o métodos que operan con tipos que se especifican posteriormente, permitiendo reutilizar código de forma segura y flexible. En lugar de escribir versiones separadas para cada tipo, se define una única versión parametrizada.

El ejemplo anterior es una forma muy básica e incompleta de programación genérica, porque permite almacenar cualquier tipo, pero no ofrece seguridad de tipos en tiempo de compilación. Por ello, se considera un enfoque débil comparado con los mecanismos modernos de generics o templates.

---

## 3. Indica los problemas respecto al chequeo de tipos, de emplear `void*` o `Object` cuando se crean estructuras de datos genéricas. 

### Respuesta:

El principal problema es la pérdida de seguridad de tipos en tiempo de compilación. El compilador no puede verificar que el tipo recuperado sea correcto, por lo que los errores se detectan únicamente en tiempo de ejecución, lo que puede causar fallos inesperados.

Además, obliga a realizar conversiones explícitas (*casting*), aumentando la complejidad del código y la probabilidad de errores. También se pierde expresividad: no queda claro qué tipo de datos se espera, lo que dificulta la lectura y mantenimiento.

---

## 4. Vamos entonces con mecanismos de mejora de la programación genérica ¿Qué son los **parámetros de tipo**? 

### Respuesta:

Los parámetros de tipo son elementos que permiten definir clases o métodos donde el tipo de dato se especifica como un parámetro. En Java se escriben como `<T>`, `<E>`, etc., y actúan como sustitutos de tipos reales.

Esto permite al compilador comprobar que se están utilizando correctamente los tipos, eliminando la necesidad de hacer *casting*. Además, mejora la claridad del código, ya que el tipo concreto queda explícito en el uso de la clase o método.

---

## 5. En Java existe "generics", en C++ existen "templates". Pon un ejemplo de uso de programación genérica en ambos, instanciando una lista o vector dinámico que solo admite `String`. Introduce valores, y luego haz un recorrido de ellos mostrando cómo cada elemento es del tipo concreto con seguridad.

### Respuesta:

En Java, se usa `List<String>` para indicar que la lista solo contiene cadenas. El compilador garantiza que no se puedan insertar otros tipos. En C++, los templates permiten crear una instancia específica de un contenedor para un tipo concreto.

Ambos enfoques mejoran la seguridad y eliminan la necesidad de casting, permitiendo trabajar directamente con tipos concretos.

Java:
```Java
import java.util.*;

List<String> lista = new ArrayList<>();
lista.add("Hola");
lista.add("Mundo");

for (String s : lista) {
    System.out.println(s.length());
}
```

C++:
```C
#include <vector>
#include <string>

std::vector<std::string> lista;
lista.push_back("Hola");
lista.push_back("Mundo");

for (auto& s : lista) {
    std::cout << s.size() << std::endl;
}
```

---

## 6. Sobre el funcionamiento de la programación genérica. ¿Qué hace el compilador cuando se instancia una clase que tiene parámetros de tipo? ¿Hace lo mismo C++ y Java? ¿Qué es el "type erasure" de Java y la "instanciación de plantillas" de C++?

### Respuesta:

En C++, el compilador realiza una **instanciación de plantillas**, generando código específico para cada tipo utilizado. Esto significa que realmente existen múltiples versiones del código, una por cada tipo concreto.

En Java, se aplica el **type erasure**, eliminando la información de tipos genéricos en tiempo de compilación y reemplazándolos por su tipo base (normalmente `Object`). El código generado es uno solo, y los tipos genéricos solo existen para el compilador.

---

## 7. Vamos a crear una nueva clase con parámetros de tipo. Define en Java una clase `Par`, que permite alojar dos valores de tipos diferentes. Incluye un constructor y un getter para cada tipo. Pon un ejemplo de uso de ese `Par`, por ejemplo para especificar el tipo de retorno de una función que devuelve en un `Par` la media y desviación típica de un array de `double`. 

### Respuesta:

Se puede definir una clase genérica `Par<T, U>` que almacena dos valores de distinto tipo. Esto permite devolver múltiples valores con tipos diferentes de forma segura y sin necesidad de usar `Object`.

El ejemplo muestra cómo utilizar esta clase para devolver dos valores calculados (media y desviación), manteniendo seguridad de tipos.

Código:
```Java
class Par<T, U> {
    private T primero;
    private U segundo;

    public Par(T primero, U segundo) {
        this.primero = primero;
        this.segundo = segundo;
    }

    public T getPrimero() { return primero; }
    public U getSegundo() { return segundo; }
}
```

---

## 8. En Java, se pueden declarar parámetros de tipo también a nivel de método, no solo a nivel de clase. Pon un ejemplo con un método genérico `seleccionaUno`, que pasados dos objetos del mismo tipo, te devuelva aleatoriamente uno de ellos. Muestra la diferencia de definirlo con dos `Object`, a definirlo con dos parámetros de tipo, en terminos de (i) evitar downcasting y (ii) forzar que ambos objetos sean del mismo tipo. 

### Respuesta


## 9. ¿Se pueden establecer restricciones en los parámetros de tipo? Por ejemplo, si quiero definir un tipo genérico `<T>`, ¿puedo decir que tenga que ser, al menos, un número para poder tratarlo como tal? Pon un ejemplo en Java de un `Punto` con dos coordenadas, metodos `getX`, `getY`, y una función `calcularDistanciaA` otro `Punto`. Permite que esas coordenadas sean cualquier tipo de número. Pon dos soluciones: una simplemente creando coordenadas de tipo `Number` y otra añadiendo generics para reforzar el chequeo de tipos y saber exactamente con qué tipo de número trabaja el `Punto`. En este caso y respecto al "type erasure", ¿cuál es el tipo final tras la compilación?

### Respuesta


## 10. Sobre las soluciones anteriores. Si bien ambas permiten trabajar con distintos tipos de número sin duplicar la clase `Punto`, reflexiona sobre el refuerzo del chequeo de tipos con generics. ¿Permiten ambas crear un punto con una coordenada de tipo entero y la otra coordenada de tipo real? ¿Qué tipo devuelve el `getX` con la solucion sin generics y qué tipo devuelve el que tiene la solución con generics?

### Respuesta


## 11. Hagamos un ejemplo avanzado. El siguiente código, con interfaz `Punto`, que define un método `calcularDistanciaA(Punto p)`, junto con las implementaciones `Punto2D` y `Punto3D`. Añade generics para asegurarnos que la sobreescritura del método calcular distancia a otro `Punto` siempre es sobre un `Punto` del mismo tipo, evitando `instanceof` y el downcasting.
```java
public interface Punto { 
    public double distanciaA(Punto p); 
} 

public class Punto2D implements Punto { 
     private final double x, y; 
     public Punto2D(double x, double y) { 
        this.x = x; this.y = y; 
    } 

    @Override 
    public double distanciaA(Punto p) { 
        if (p instanceof Punto2D) { 
            Punto2D p2d = (Punto2D) p; 
            return Math.sqrt(Math.pow(x - p2d.x, 2) 
                    + Math.pow(y - p2d.y, 2)); 
        } else { 
            throw new RuntimeException("p debe ser Punto 2D"); 
        } 
    } 
} 
public class Punto3D implements Punto { 
    // Igual que Punto2D, pero con tres coordenadas
    ...
} 
```

### Respuesta


## 12. Dado que `String` es subtipo de `Object`, ¿significa eso que `List<String>` es subtipo de `List<Object>`? ¿Y que `String[]` es subtipo de `Object[]`? Razona por qué la respuesta es diferente en cada caso y qué problema en tiempo de ejecución puede aparecer con los arrays. A partir de estos ejemplos, define qué significa que un tipo genérico sea **covariante**, **contravariante** o **invariante** respecto a su parámetro de tipo.

### Respuesta


## 13. Java permite recuperar covarianza y contravarianza en tipos genéricos de forma controlada mediante **wildcards**. ¿Qué es un wildcard (`?`)? Muestra la diferencia entre `List<? extends T>` y `List<? super T>`, indicando en qué casos se usa cada uno. Pon dos ejemplos: (i) un método que reciba una lista de números y calcule su suma, usando `? extends`; (ii) un método que reciba una lista y le añada varios números enteros, usando `? super`.

### Respuesta
