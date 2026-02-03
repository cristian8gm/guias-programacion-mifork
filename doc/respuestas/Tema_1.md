<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Clases y Objetos". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: ninguno.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->

# TEMA 1. Clases y objetos

## 1. ¿Cuáles son las cuatro características básicas de la programación orientada a objetos? Describe brevemente cada una

### Respuesta: 
La programación orientada a objetos se basa en cuatro pilares fundamentales: **abstracción**, **encapsulación**, **herencia** y **polimorfismo**. La abstracción permite representar elementos del mundo real mediante modelos simplificados y centrados únicamente en los detalles esenciales. Este mecanismo ayuda a gestionar la complejidad de los sistemas al ocultar aquello que no es relevante para el objetivo del programa.

La encapsulación consiste en ocultar el estado interno de un objeto y ofrecer acceso controlado a través de métodos. Esto reduce errores y asegura la coherencia de los datos. La herencia permite crear nuevas clases a partir de otras ya existentes, reutilizando atributos y comportamientos. Por último, el polimorfismo posibilita que diferentes objetos respondan de manera distinta a un mismo método, otorgando flexibilidad y adaptabilidad al diseño del software.

---

## 2. Cita cuatro lenguajes populares que permitan la programación orientada a objetos

### Respuesta: 
Existen numerosos lenguajes que soportan la programación orientada a objetos. Uno de los más representativos es **Java**, diseñado específicamente con la orientación a objetos como núcleo fundamental. Otro lenguaje muy extendido es **C++**, que combina características estructuradas procedentes del lenguaje C con capacidades completas de orientación a objetos.

También destaca **Python**, un lenguaje multiparadigma donde prácticamente todo se trata como un objeto, lo que facilita aprender progresivamente la filosofía de la POO. Finalmente, **C#** es otro lenguaje ampliamente utilizado en el entorno Microsoft, con una sintaxis y enfoque muy similares a los de Java y con un fuerte soporte para la orientación a objetos.

---

## 3. Los paradigmas anteriores a la POO, ¿Qué es la **programación estructurada**? y, todavía mejor, ¿Qué es la **programación modular**?

### Respuesta: 
La **programación estructurada** es un paradigma que organiza el código en bloques lógicos mediante estructuras de control bien definidas, como secuencias, condicionales y bucles. Su objetivo principal es promover un flujo de ejecución claro y reducir el uso de instrucciones como `goto`, que generan código difícil de mantener. Este enfoque favorece la claridad y la reducción de errores.

La **programación modular** amplía esta idea dividiendo el programa en partes independientes llamadas módulos. Cada módulo agrupa funciones y datos relacionados, lo que facilita el desarrollo, prueba y mantenimiento del software. Este enfoque fue un paso importante hacia la orientación a objetos, ya que ambos paradigmas buscan dividir el programa en entidades más pequeñas, comprensibles y reutilizables.

---

## 4. ¿Qué tres elementos definen a un objeto en programación orientada a objetos?

### Respuesta: 
Un objeto se define por tres elementos fundamentales: **estado**, **comportamiento** e **identidad**. El estado está compuesto por los valores actualizados de sus atributos, que describen cómo se encuentra el objeto en un momento determinado. Estos atributos reflejan la información relevante que el programa necesita gestionar.

El comportamiento de un objeto se expresa a través de sus métodos, que permiten modificar su estado, consultar información o interactuar con otros objetos. Finalmente, la identidad distingue a un objeto de cualquier otro, incluso aunque ambos compartan el mismo estado y el mismo comportamiento. Cada objeto es una entidad única dentro del programa debido a esta identidad propia.

---

## 5. ¿Qué es una clase? ¿Es lo mismo que un objeto? ¿Qué es una instancia? ¿Todos los lenguajes orientados a objetos manejan el concepto de clase?

### Respuesta: 
Una **clase** es un modelo o plantilla que define los atributos y métodos que tendrán los objetos creados a partir de ella. Funciona como un molde que describe qué datos y comportamientos compartirán todas las instancias basadas en esa clase. Esto permite crear tipos personalizados que organizan la lógica del programa de manera clara y estructurada.

Un objeto no es lo mismo que una clase. Un objeto es una **instancia** concreta de una clase, creada en memoria y con valores propios en sus atributos. Cada instancia es independiente, aunque todas se basan en la misma definición proporcionada por la clase. No todos los lenguajes orientados a objetos utilizan estrictamente el concepto de clase: lenguajes como Java y C++ sí lo hacen, mientras que otros como JavaScript utilizan un modelo basado en prototipos, donde los objetos se crean a partir de otros objetos sin necesidad de clases formales.

---

## 6. ¿Dónde se almacenan en memoria los objetos? ¿Es igual en todos los lenguajes? ¿Qué es la **recolección de basura**? 

### Respuesta:
El lugar donde se almacenan los objetos depende del lenguaje y de su modelo de memoria. En lenguajes como Java, todos los objetos se almacenan en el *heap*, mientras que las variables que los referencian suelen estar en la pila (*stack*). Esto implica que las variables contienen únicamente referencias al objeto real, que está en memoria dinámica. Esta separación facilita que los objetos tengan un ciclo de vida independiente del ámbito donde se declaran las referencias.

No todos los lenguajes funcionan igual. En C++, por ejemplo, los objetos pueden estar tanto en la pila como en el *heap*, dependiendo de cómo se declaren. Si se crea un objeto sin `new`, se almacena en la pila y se destruye automáticamente al salir del ámbito. Si se crea con `new`, reside en el *heap* y debe ser liberado manualmente con `delete`. Este manejo manual hace al lenguaje más flexible, pero también más propenso a errores.

La *recolección de basura* (garbage collection) es un mecanismo automático que se encarga de liberar la memoria de objetos que ya no se utilizan. Java utiliza este sistema para evitar fugas de memoria, de modo que el programador no necesita realizar operaciones explícitas para liberar memoria. Aunque simplifica el desarrollo, también implica que no se tiene un control absoluto sobre cuándo se libera la memoria.

---

## 7. ¿Qué es un método? ¿Qué es la **sobrecarga de métodos**? 

### Respuesta:
Un método es una función que pertenece a una clase y define comportamientos que los objetos de esa clase pueden realizar. A diferencia de las funciones en C, los métodos tienen acceso directo al estado interno del objeto, lo cual permite modificar sus atributos o consultar su información. Esto refuerza la idea de que un objeto combina datos y el comportamiento asociado a esos datos.

La sobrecarga de métodos consiste en definir varios métodos con el mismo nombre dentro de una misma clase, siempre que tengan distintos parámetros. Esta característica permite que un mismo nombre represente acciones similares pero adaptadas a distintos tipos o cantidades de datos. Es una forma de polimorfismo en tiempo de compilación, que mejora la legibilidad y el diseño del código.

---

## 8. Ejemplo mínimo de clase en Java, que se llame Punto, con dos atributos, x e y, con un método que se llame `calculaDistanciaAOrigen`, que calcule la distancia a la posición 0,0. Por sencillez, los atributos deben tener visibilidad por defecto. Crea además un ejemplo de uso con una instancia y uso del método

### Respuesta:
A continuación se muestra una clase sencilla llamada `Punto`, con dos atributos `x` e `y` y un método para calcular la distancia al origen:

```java
public class Punto {
    int x;
    int y;

    double calculaDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }
}
```

---

## 9. ¿Cuál es el punto de entrada en un programa en Java? ¿Qué es `static` y para qué vale? ¿Sólo se emplea para ese método `main`? ¿Para qué se combina con `final`?

### Respuesta:
El punto de entrada de un programa en Java es el método main, cuya firma estándar es public static void main(String[] args). Este método es llamado automáticamente por la máquina virtual Java al iniciar el programa. Su firma es fija porque la JVM necesita reconocerlo exactamente para poder ejecutarlo.
El modificador static indica que el método pertenece a la clase y no a una instancia concreta. Esto permite ejecutar main sin necesidad de crear un objeto previamente. El uso de static no se limita al método main; puede aplicarse también a métodos o atributos que deban ser compartidos por todas las instancias de una clase.
Cuando static se combina con final, el resultado suele ser una constante. En ese caso, static indica que la constante es única para toda la clase y final que su valor no puede modificarse. Esto se utiliza frecuentemente para valores fijos como configuraciones o constantes matemáticas.

---

## 10. Intenta ejecutar un poco de Java de forma básica, con los comandos `javac` y `java`. ¿Cómo podemos compilar el programa y ejecutarlo desde linea de comandos? ¿Java es compilado? ¿Qué es la **máquina virtual**? ¿Qué es el *byte-code* y los ficheros `.class`?

### Respuesta:
Para compilar un archivo Java desde la línea de comandos se utiliza el comando javac, seguido del nombre del archivo fuente. Por ejemplo, javac Principal.java generará uno o varios archivos .class que contienen el bytecode. Una vez compilado, el programa se ejecuta con el comando java, indicando el nombre de la clase que contiene el método main, sin la extensión .class.
Java es un lenguaje que se considera a la vez compilado e interpretado. Se compila primero a bytecode, un código intermedio independiente del sistema operativo, y posteriormente este bytecode es ejecutado por la máquina virtual Java (JVM). La JVM actúa como una capa intermedia que interpreta o compila dinámicamente el bytecode y lo adapta al sistema en el que se ejecuta.
Los archivos .class son precisamente los ficheros que almacenan ese bytecode. Gracias a este formato, el mismo programa puede ejecutarse en múltiples plataformas siempre que exista una JVM compatible, lo que convierte a Java en un lenguaje altamente portable.

---

## 11. En el código anterior de la clase `Punto` ¿Qué es `new`? ¿Qué es un **constructor**? Pon un ejemplo de constructor en una clase `Empleado` que tenga DNI, nombre y apellidos

### Respuesta:

La palabra clave `new` en Java sirve para crear un nuevo objeto en memoria dinámica (heap). Cuando se usa, el lenguaje reserva espacio para la instancia y devuelve una referencia que se puede almacenar en una variable. A diferencia de C++, en Java no es posible crear objetos sin `new`, ya que todas las instancias deben alojarse en memoria dinámica mediante este mecanismo.

Un constructor es un método especial que se ejecuta automáticamente al crear un objeto. Su función principal es inicializar los atributos del objeto, asignándoles valores iniciales apropiados. El constructor tiene exactamente el mismo nombre que la clase y no declara ningún tipo de retorno, lo que permite distinguirlo de otros métodos.
```java
public class Empleado {
    String dni;
    String nombre;
    String apellidos;

    public Empleado(String dni, String nombre, String apellidos) {
        this.dni = dni;
        this.nombre = nombre;
        this.apellidos = apellidos;
    }
}
```

---

## 12. ¿Qué es la referencia `this`? ¿Se llama igual en todos los lenguajes? Pon un ejemplo del uso de `this` en la clase `Punto`

### Respuesta:
La palabra clave `this` es una referencia al propio objeto sobre el que se está ejecutando un método. Permite acceder de manera explícita a los atributos o métodos de la instancia, especialmente cuando existen variables locales con el mismo nombre que los atributos. También se utiliza para llamar a otros constructores de la misma clase.

En otros lenguajes orientados a objetos existe un concepto equivalente, aunque con nombres distintos. En C++ también se llama `this`, mientras que en Python se utiliza `self`. En todos los casos, su función es señalar al objeto actual dentro del contexto de un método.

Ejemplo práctico en `Punto`:

```java
double calculaDistanciaAOrigen() {
    return Math.sqrt(this.x * this.x + this.y * this.y);
}
```

---

## 13. Añade ahora otro nuevo método que se llame `distanciaA`, que reciba un `Punto` como parámetro y calcule la distancia entre `this` y el punto proporcionado

### Respuesta:

Para calcular la distancia entre dos puntos en un plano, puede utilizarse el teorema de Pitágoras. El método recibe un objeto `Punto` como parámetro y compara sus coordenadas con las del objeto actual (al que se accede con `this`). Este tipo de interacción entre objetos es habitual en la programación orientada a objetos.

El método podría implementarse así:

```java
double distanciaA(Punto otro) {
    int dx = this.x - otro.x;
    int dy = this.y - otro.y;
    return Math.sqrt(dx * dx + dy * dy);
}
```

---

## 14. El paso del `Punto` como parámetro a un método, es **por copia** o **por referencia**, es decir, si se cambia el valor de algún atributo del punto pasado como parámetro, dichos cambios afectan al objeto fuera del método? ¿Qué ocurre si en vez de un `Punto`, se recibiese un entero (`int`) y dicho entero se modificase dentro de la función? 

### Respuesta:

En Java, todos los parámetros se pasan **por copia**, pero en el caso de los objetos, lo que se copia es **la referencia**, no el objeto en sí. Esto significa que si dentro del método se modifica un atributo del objeto, dicho cambio también se verá fuera del método, ya que ambas variables apuntan al mismo objeto en memoria.

Sin embargo, si dentro del método se reasigna la referencia (por ejemplo, `otro = new Punto();`), esta nueva asignación no afecta fuera del método porque la copia de la referencia solo existe dentro del ámbito del método. Con los tipos primitivos como `int`, la copia es del valor, por lo que cualquier modificación interna no afectará al valor original en el exterior.

---

## 15. ¿Qué es el método `toString()` en Java? ¿Existe en otros lenguajes? Pon un ejemplo de `toString()` en la clase `Punto` en Java

### Respuesta:

El método `toString()` es un método heredado de la clase base `Object` cuyo propósito es devolver una representación textual del objeto. Cuando se llama a `System.out.println(objeto);`, Java invoca automáticamente el método `toString()` para mostrar una cadena que representa al objeto. Sobrescribirlo permite controlar cómo se muestra su información interna.

Este método tiene equivalentes en otros lenguajes: en Python se emplea `__str__`, mientras que en C# se utiliza `ToString()`. Aunque varíe el nombre, el objetivo es el mismo: transformar el objeto en una cadena legible para el usuario.

Ejemplo de `toString()` en la clase `Punto`:

```java
@Override
public String toString() {
    return "(" + this.x + ", " + this.y + ")";
}
```

---

## 16. Reflexiona: ¿una clase es como un `struct` en C? ¿Qué le falta al `struct` para ser como una clase y las variables de ese tipo ser instancias?


### Respuesta:

Un `struct` en C se parece a una clase en el sentido de que permite agrupar varios datos en un único tipo. Esta similitud hace que a veces se lo considere un precursor de las clases modernas. Sin embargo, un `struct` en C solo puede contener datos y no métodos asociados, por lo que carece de comportamiento propio.

Además, el `struct` no soporta conceptos fundamentales de la programación orientada a objetos, como encapsulación, herencia o polimorfismo. Todo el comportamiento debe implementarse mediante funciones externas, que operan sobre los datos pasados como puntero. Por ello, aunque comparten la idea de agrupar información, un `struct` no puede funcionar como una clase completa.

---

## 17. Quitemos un poco de magia a todo esto: ¿Como se podría “emular”, con `struct` en C, la clase `Punto`, con su función para calcular la distancia al origen? ¿Qué ha pasado con `this`?

### Respuesta:

Para emular una clase en C, puede definirse un `struct` que contenga los datos del punto y luego escribir funciones externas que operen sobre él. Como C no admite métodos dentro de estructuras, es necesario pasar el `struct` como puntero a cada función que desee trabajar con sus datos. Este mecanismo recuerda de forma distante a los métodos, pero sin el soporte automático que proporcionan los lenguajes orientados a objetos.

Un ejemplo aproximado sería:

```c
typedef struct {
    int x;
    int y;
} Punto;

double calculaDistanciaAOrigen(Punto* p) {
    return sqrt(p->x * p->x + p->y * p->y);
}
```