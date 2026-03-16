<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Excepciones". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 3. Excepciones

## 1. Empecemos un tema sobre control de errores en lenguajes de programación, con algo básico. En C, donde no existen las excepciones, pongamos un ejemplo de una raíz que toma número flotante positivo. Queremos controlar el error si la función recibe un número negativo. El usuario debe ser informado pero desde fuera de la función `raiz` ¿Cómo indicamos ese error?. Enumera dos opciones diferentes de diseñar, poniendo un ejemplo de código de cada una.

### Respuesta:

En C, al no existir un mecanismo de excepciones, el control de errores suele llevarse a cabo mediante códigos devueltos o valores especiales. Una opción común consiste en devolver un valor indicador que denote error, como `-1.0`, y permitir que el llamador verifique si dicho valor significa que se ha producido un problema. Esta estrategia es sencilla pero puede causar ambigüedad si el valor usado para indicar error pudiera ser también un resultado legítimo.

Otra alternativa consiste en utilizar parámetros por referencia para devolver el resultado y un valor entero que indique el estado (éxito o fallo). Con este enfoque, la función puede devolver un código de error específico, mientras el resultado se almacena solo si es válido. Esta opción permite separar claramente la señalización del error del valor calculado, facilitando un control más preciso desde fuera de la función.

Código (opción 1: valor especial):
```c
double raiz(double x) {
    if (x < 0.0) {
        return -1.0; // error
    }
    return sqrt(x);
}
```
Código (opción 2: código de estado por referencia):
```c
int raiz(double x, double *resultado) {
    if (x < 0.0) return -1;  // error
    *resultado = sqrt(x);
    return 0;
}
```

---

## 2. Brevemente ¿Qué es una **"excepción"**? ¿Con qué objetivo las usa un programador cuando implementa funciones o cuando las llama?

### Respuesta:

Una excepción es un mecanismo que permite señalar y gestionar errores o situaciones anómalas durante la ejecución de un programa. En lugar de regresar a la función llamadora con códigos de error, la ejecución normal se interrumpe y se transfiere el control a un manejador de excepciones adecuado. Este mecanismo permite separar la lógica normal del código de tratamiento de errores.

Los programadores utilizan excepciones para notificar automáticamente fallos que no pueden resolverse en el punto donde se detectan. Para quien llama, este mecanismo ofrece una forma clara de reaccionar ante errores sin mezclar comprobaciones constantes dentro del flujo normal. Además, la propagación automática simplifica el manejo de situaciones inesperadas y evita que los errores queden silenciados.

---

## 3. Reescribe el mismo ejemplo de raiz, pero en Java, metiendo ese método en una clase `Calculadora` y llama a dicho método desde el método `main`, mostrando cómo se puede controlar desde fuera.

### Respuesta:

En Java se puede representar el error lanzando una excepción cuando el número es negativo. Dentro de la clase `Calculadora`, el método `raiz` verificará el argumento y, si no es válido, lanzará una `IllegalArgumentException`. De este modo, la responsabilidad de manejar el error recaerá en el código llamador, que podrá capturarla mediante un bloque `try-catch`.

En el método `main`, se llamará al método `raiz` desde un bloque `try`, y si la entrada es incorrecta, se capturará la excepción mostrando un mensaje informativo. Esto separa claramente las responsabilidades: la función valida y notifica; el llamador decide qué hacer ante la situación anómala.

Código:
```Java
class Calculadora {
    public static double raiz(double x) {
        if (x < 0) {
            throw new IllegalArgumentException("Número negativo.");
        }
        return Math.sqrt(x);
    }

    public static void main(String[] args) {
        try {
            double r = raiz(-5);
            System.out.println("Resultado: " + r);
        } catch (IllegalArgumentException e) {
            System.out.println("Error desde fuera: " + e.getMessage());
        }
    }
}
```

---

## 4. ¿Qué es **"lanzar"** una excepción? ¿Qué es **"controlar"** o **"capturar"** una excepción? ¿Qué es que se **"propague"** una excepción? ¿Qué le va ocurriendo a las funciones en la pila de llamadas por donde se va propagando la excepción? ¿Las funciones que no la controlan se reanudan después de alguna forma? Explica con el mismo ejemplo anterior en Java de la raíz cuadrada.

### Respuesta:

Lanzar una excepción significa interrumpir el flujo normal de ejecución y crear un objeto que describe la situación de error. En Java se hace con la palabra clave `throw`. Una vez lanzada, la excepción sube por la pila en busca de un bloque `catch` que coincida con su tipo. Controlar o capturar una excepción significa manejarla en dicho bloque mediante código capaz de responder al problema.

La propagación implica que, si una función no captura la excepción, esta continúa ascendiendo hacia la función que la llamó. Cada marco de la pila se descarta, liberando el contexto y sin reanudar ejecución tras el punto donde ocurrió el error. Por esa razón, en el ejemplo de `raiz`, si `main` captura la excepción y `raiz` no, `raiz` no se reanuda tras lanzar la excepción; simplemente deja de ejecutarse al abandonar su marco.

---

## 5. ¿Qué ventajas tiene frente a C, la **"propagación natural"** de las excepciones a través de la pila (*stack*) de llamadas?

### Respuesta:

La propagación natural evita que cada función tenga que realizar comprobaciones explícitas de retorno como ocurre en C. Esto reduce considerablemente el código repetitivo y las estructuras condicionales, mejorando la claridad del programa y separando responsabilidades. Además, permite que la gestión del error se realice en el nivel adecuado, sin ensuciar las funciones intermedias.

Otra ventaja significativa es la seguridad de que los errores no se silencian accidentalmente. En C, si un programador olvida comprobar un código de retorno, el fallo puede pasar inadvertido y causar efectos no deseados. En Java, si nadie captura la excepción, el programa finaliza mostrando un mensaje detallado, lo que facilita la localización del problema.

---

## 6. En orientación a objetos, ¿las excepciones suelen ser objetos? ¿Qué ventajas tiene esto en términos de encapsulación? ¿Podemos entonces crear excepciones personalizadas?

### Respuesta:

Sí, en lenguajes orientados a objetos como Java las excepciones se representan mediante objetos. Esto permite que las excepciones encapsulen información útil respecto a la causa, el mensaje explicativo y, si se desea, datos adicionales relacionados con el error. Esta estructura facilita la comunicación clara de problemas entre módulos y mejora la mantenibilidad del código.

El hecho de ser objetos permite extender clases base y crear excepciones personalizadas que modelen situaciones específicas del dominio del programa. Un programador puede definir su propia clase de excepción con campos adicionales o comportamientos particulares, reforzando así la expresividad del sistema y manteniendo la coherencia con el diseño orientado a objetos.

---

## 7. En relación con las ventajas de la encapsulación, comparando el ejemplo en C con Java. ¿Qué **información esencial** lleva cualquier **objeto excepción** que es muy útil tener cuando se llega a un manejador?

### Respuesta:

Los objetos excepción incluyen típicamente un mensaje descrito por una cadena que explica la causa del error, lo cual es más expresivo que devolver un simple código numérico como en C. Además, suelen contener la traza de pila (stack trace), que indica la secuencia exacta de llamadas que condujeron al problema, ayudando en gran medida al diagnóstico.

También pueden incluir información contextual adicional, como la causa raíz (otra excepción) o datos específicos del dominio, si se trata de una excepción personalizada. Esto permite tomar decisiones más informadas al manejar el error, mejorando significativamente la depuración y el mantenimiento del programa en comparación con la versión en C.

---

## 8. En Java, sobre el bloque **"try-catch"**, ¿se pueden tener más de un bloque `catch`? ¿cuántos bloques `catch` se ejecutan?

### Respuesta:

Es posible tener múltiples bloques `catch` asociados a un mismo `try`, cada uno diseñado para interceptar excepciones de tipos distintos. Esto permite diferenciar entre diversas clases de error y reaccionar apropiadamente a cada una sin mezclar la lógica. El orden de los `catch` es importante, ya que se evalúan secuencialmente según aparecen.

A pesar de poder tener varios `catch`, solo uno se ejecuta por cada excepción lanzada. Una vez que un `catch` coincide con el tipo de la excepción (o con un supertipo adecuado), se ejecuta su contenido y se ignoran los demás. Esto asegura un control claro y evita ambigüedades en la gestión del error.

---

## 9. Si las excepciones producen rupturas en el código llamador, ¿cómo podemos garantizar que se ejecuta siempre finalmente un código necesario para cierre de ficheros, liberacion de recursos, antes de que continúe propagándose la excepción? Pon un ejemplo en Java con `finally`, tanto con `catch` como sin él.

### Respuesta:

El bloque `finally` se emplea para garantizar que cierto código se ejecute independientemente de si ocurre o no una excepción. Esto resulta esencial para liberar recursos como ficheros, conexiones o memoria, ya que la interrupción provocada por una excepción podría impedir su cierre si no se planifica adecuadamente. El `finally` asegura la limpieza incluso cuando una excepción se propaga.

Un `finally` puede acompañar a un `try-catch`, ejecutándose después del `catch`, o puede combinarse con un `try` sin `catch`, en cuyo caso se ejecutará antes de que la excepción continúe propagándose. Este comportamiento lo hace especialmente útil en patrones de gestión de recursos.

Con `catch`:
```Java
try {
    abrirFichero();
} catch (IOException e) {
    System.out.println("Error al abrir fichero.");
} finally {
    cerrarFichero();
}
```

Sin `catch`:
```Java
try {
    abrirFichero();
    leerFichero();
} finally {
    cerrarFichero();
}
```

---

## 10. En Java, el bloque `finally` puede ir sin `catch`? ¿Se ejecuta siempre tanto si ocurre como si no ocurre una excepción? ¿Y si hay un `return` en medio del `try`?

### Respuesta:

El bloque `finally` puede aparecer sin necesidad de un bloque `catch`, formando la estructura `try-finally`. En estos casos, si dentro del `try` ocurre una excepción, el `finally` se ejecuta antes de que la excepción continúe su propagación hacia fuera. Esto lo hace útil cuando se desea limpieza de recursos sin capturar el error.

El `finally` se ejecuta incluso si no ocurre ninguna excepción, o si hay un `return` dentro del `try`. La especificación de Java garantiza que el `finally` tenga prioridad, ejecutándose justo antes de que la función retorne o la excepción se propague. Solo situaciones extremas, como finalizar la JVM, impedirían su ejecución.

---

## 11. En Java, qué son las excepciones **"controladas"** y las **"no controladas"**? ¿Qué papel juega `RuntimeException`? Pon un ejemplo de excepciones típicas controladas y no controladas que incluso nosotros mismos podríamos usar. Haz dos listas con 3 o 4 ejemplos de situación donde se suele preferir una excepción controlada y donde se suele preferir una excepción no controlada.

### Respuesta:

Las excepciones controladas (checked) son aquellas que el compilador obliga a tratar explícitamente, ya sea capturándolas o declarándolas con `throws`. Se usan cuando la situación de error es esperada y forma parte del contrato del método. Las no controladas (unchecked), derivadas de `RuntimeException`, se utilizan para errores de programación o situaciones que no se pretende forzar a manejar de inmediato.

`RuntimeException` es la clase base de las no controladas. Excepciones típicas controladas incluyen `IOException`, `SQLException` o una excepción personalizada como `DatosNoDisponiblesException`. Unchecked típicas son `IllegalArgumentException`, `NullPointerException`, `ArithmeticException` o una personalizada `ConfiguracionInvalidaException`.

Situaciones para preferir excepciones controladas:
1. Lectura de ficheros.
2. Comunicación con red.
3. Operaciones que dependen de recursos externos.
4. Transformaciones que pueden fallar de forma esperada.

Situaciones para preferir excepciones no controladas:
1. Argumentos inválidos en métodos.
2. Llamadas en estados imposibles del programa.
3. Violación de invariantes internas.
4. Errores de programación como índices fuera de rango.

---

## 12. ¿Qué es y para qué se usa `throws`? ¿Por qué es alternativa a capturar una excepción controlada?

### Respuesta:

La palabra clave `throws` se utiliza en la firma de un método para declarar que puede lanzar una excepción controlada. Indica al llamador que este método no manejará el error y que la responsabilidad de capturarlo o permitir que continúe la propagación recae en quien invoca. Esta declaración forma parte del contrato público del método.

Es una alternativa a capturar la excepción dentro del método cuando el programador considera que no posee suficiente información para actuar sobre el error. En lugar de forzar un manejo local incorrecto, se permite que niveles superiores del programa tomen la decisión adecuada.

---

## 13. Pon un ejemplo en Java de firma de método que incluya `throws`, de una función que abre un fichero pero que declara que no le interesa menejar la excepción de si el fichero no existe, sino que se propague hacia arriba. Eso sí, acuérdate del `finally`.

### Respuesta:

Este método declara que puede lanzar `IOException`, dejando claro que el error debe gestionarse desde niveles superiores. Dentro del cuerpo no se usa un bloque `catch`, sino solo un `try-finally` que garantiza el cierre del recurso. De esta forma, se asegura la limpieza sin absorber el error.

Esta técnica es común en bibliotecas que acceden a ficheros donde la decisión final sobre el manejo del error debe corresponder al usuario de la API. Así se separan adecuadamente las responsabilidades entre niveles del sistema.

Código:
```Java
public void procesarArchivo(String nombre) throws IOException {
    FileInputStream fis = null;
    try {
        fis = new FileInputStream(nombre); // puede lanzar IOException
        // procesamiento...
    } finally {
        if (fis != null) {
            fis.close();
        }
    }
}
```

---

## 14. ¿Podemos poner en `throws` excepciones no controladas, como `RuntimeException`? ¿Debería el método llamador entonces poner `try-catch` en ese caso? ¿Qué sentido tendría?

### Respuesta:

Es posible incluir excepciones no controladas en la cláusula `throws`, aunque no es obligatorio. Dado que el compilador no obliga a tratarlas, su declaración sirve más como documentación para los usuarios del método, indicando que ciertas condiciones pueden generar errores en tiempo de ejecución.

El método llamador no tiene por qué capturarlas salvo que tenga una estrategia concreta para responder a ellas. En general, se capturan excepciones no controladas solo en situaciones donde se desea registrar el error, transformar la excepción o garantizar la liberación de recursos antes de abortar la operación.

---

## 15. ¿Cuándo se recomienda usar excepciones controladas, como `IOException`, y cuándo no controladas como `IllegalArgumentException`? ¿Existen en todos los lenguajes ambas opciones? En los que sólo existe una opción, ¿cuál es la más habitual?

### Respuesta:

Las excepciones controladas se recomiendan cuando la situación es esperable y parte del contrato del método, como acceso a ficheros, errores de red o cualquier operación que interactúa con el exterior del programa. Las no controladas se usan para errores lógicos del programa, violaciones de precondiciones o estados imposibles que indican fallos de diseño.

No todos los lenguajes distinguen entre controladas y no controladas. Algunos, como Python o C++, solo usan excepciones no controladas, privilegiando la flexibilidad y evitando sobrecargar al programador con declaraciones obligatorias. En estos casos, la excepción no controlada es la opción habitual.

---

## 16. ¿Tiene sentido lanzar excepciones dentro del `catch`? ¿Se puede relanzar la misma excepción capturada? ¿Cuándo tendría sentido hacer esto último? Pon ejemplos de ambos casos.

### Respuesta:

Lanzar una excepción dentro del `catch` puede tener sentido cuando el manejador local no puede resolver la situación, pero es necesario transformar la excepción en otra más adecuada para niveles superiores. Esta técnica permite encapsular detalles internos y mejorar la claridad del contrato del método.

También es posible relanzar la misma excepción capturada utilizando `throw e;`. Esto resulta útil cuando se desea realizar ciertas acciones locales, como registrar el error, pero después se quiere permitir que la excepción continúe propagándose. Así, se conserva la información relevante y se ejecuta lógica adicional sin ocultar el problema.

Lanzar otra excepción:
```Java
catch (IOException e) {
    throw new MiExcepcionDeAltoNivel("Fallo en lectura", e);
}
```

Relanzar la misma excepción:
```Java
catch (IOException e) {
    log(e);
    throw e;
}
```

---

## 17. ¿En qué consiste que una excepción sea la **"causa"** de otra excepción? Pon un ejemplo en Java, donde capturemos una excepción de bajo nivel y la encapsulemos en otra personalizada de alto nivel. Cuando una excepción sale por pantalla y tiene una causa, ¿se ve?

### Respuesta:

Que una excepción sea la causa de otra significa que la excepción de alto nivel incluye dentro de sí a otra excepción que originó el problema. En Java, esto se logra pasando la excepción original como segundo argumento en el constructor de la nueva excepción. De este modo, se preserva la información completa del error original mientras se expone un contexto más alto y significativo.

Al imprimir una excepción con causa, la salida estándar muestra ambas trazas: la traza de la excepción principal y, a continuación, la de la excepción encadenada. Esto facilita el diagnóstico, ya que se ve claramente la relación entre el error de bajo nivel y el contexto en el que se produjo el fallo de alto nivel.

Ejemplo:
```Java
try {
    leerFichero();
} catch (IOException e) {
    throw new MiExcepcionDeAltoNivel("Error al procesar datos", e);
}
```
