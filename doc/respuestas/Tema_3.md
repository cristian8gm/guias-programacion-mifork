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

### Respuesta


## 7. En relación con las ventajas de la encapsulación, comparando el ejemplo en C con Java. ¿Qué **información esencial** lleva cualquier **objeto excepción** que es muy útil tener cuando se llega a un manejador?

### Respuesta


## 8. En Java, sobre el bloque **"try-catch"**, ¿se pueden tener más de un bloque `catch`? ¿cuántos bloques `catch` se ejecutan?

### Respuesta


## 9. Si las excepciones producen rupturas en el código llamador, ¿cómo podemos garantizar que se ejecuta siempre finalmente un código necesario para cierre de ficheros, liberacion de recursos, antes de que continúe propagándose la excepción? Pon un ejemplo en Java con `finally`, tanto con `catch` como sin él.

### Respuesta


## 10. En Java, el bloque `finally` puede ir sin `catch`? ¿Se ejecuta siempre tanto si ocurre como si no ocurre una excepción? ¿Y si hay un `return` en medio del `try`?

### Respuesta


## 11. En Java, qué son las excepciones **"controladas"** y las **"no controladas"**? ¿Qué papel juega `RuntimeException`? Pon un ejemplo de excepciones típicas controladas y no controladas que incluso nosotros mismos podríamos usar. Haz dos listas con 3 o 4 ejemplos de situación donde se suele preferir una excepción controlada y donde se suele preferir una excepción no controlada.

### Respuesta


## 12. ¿Qué es y para qué se usa `throws`? ¿Por qué es alternativa a capturar una excepción controlada?

### Respuesta


## 13. Pon un ejemplo en Java de firma de método que incluya `throws`, de una función que abre un fichero pero que declara que no le interesa menejar la excepción de si el fichero no existe, sino que se propague hacia arriba. Eso sí, acuérdate del `finally`.

### Respuesta


## 14. ¿Podemos poner en `throws` excepciones no controladas, como `RuntimeException`? ¿Debería el método llamador entonces poner `try-catch` en ese caso? ¿Qué sentido tendría?

### Respuesta


## 15. ¿Cuándo se recomienda usar excepciones controladas, como `IOException`, y cuándo no controladas como `IllegalArgumentException`? ¿Existen en todos los lenguajes ambas opciones? En los que sólo existe una opción, ¿cuál es la más habitual?

### Respuesta


## 16. ¿Tiene sentido lanzar excepciones dentro del `catch`? ¿Se puede relanzar la misma excepción capturada? ¿Cuándo tendría sentido hacer esto último? Pon ejemplos de ambos casos.

### Respuesta


## 17. ¿En qué consiste que una excepción sea la **"causa"** de otra excepción? Pon un ejemplo en Java, donde capturemos una excepción de bajo nivel y la encapsulemos en otra personalizada de alto nivel. Cuando una excepción sale por pantalla y tiene una causa, ¿se ve?

### Respuesta

