<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Polimorfismo". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación, Excepciones, Composición y Herencia.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# Tema 5. Polimorfismo

## 1. Brevemente, ¿qué es el **"polimorfismo"** y para qué sirve en programación orientada a objetos? ¿qué es la **"sobreescritura"** de métodos?

### Respuesta:

El polimorfismo es la capacidad que tienen los objetos de distintas clases relacionadas por herencia de responder de forma distinta a la misma llamada a un método. Es decir, se puede usar una referencia de tipo general (superclase) y que, en tiempo de ejecución, el comportamiento dependa del tipo real del objeto. Esto permite escribir código más general, flexible y extensible.

La **sobreescritura** (overriding) consiste en que una subclase redefine un método heredado de la superclase, manteniendo la misma firma, pero cambiando su implementación. Gracias a esto, cada subtipo puede especializar el comportamiento sin cambiar la forma en la que se invoca el método.

---

## 2. ¿En qué consiste la **"ligadura dinámica"** o **"enlace tardío"**? ¿qué relación tiene con el polimorfismo? ¿hay que indicarlos explícitamente al programar o depende esto del lenguaje? Compara C++ y Java. Indicalo después también para Python.

### Respuesta:

La ligadura dinámica o enlace tardío significa que la decisión de qué método ejecutar no se toma en tiempo de compilación, sino en tiempo de ejecución, en función del tipo real del objeto. Esto es lo que permite que el polimorfismo funcione correctamente.

En Java, este mecanismo es automático para métodos no estáticos: no hay que indicarlo explícitamente. En C++, en cambio, es necesario usar la palabra clave `virtual` para indicar que un método se resolverá dinámicamente. En Python, todo es dinámico por naturaleza, por lo que el polimorfismo se aplica sin necesidad de declaraciones específicas.

---

## 3. Pon un ejemplo sencillo en Java, de un `Soldado`, con un método `saluda`, con dos subclases: `Zapador` y `Artillero`, donde `Zapador` sobreescribe el método `saludar`, sustituyendo por completo su comportamiento. Ilustra el funcionamiento del polimorfismo creando un array de `Soldados` de dos tipos y luego recorriéndolo empleando referencias de tipo `Soldado` y llamando a `saludar`.

### Respuesta:

Se crea una jerarquía donde `Soldado` define un método `saludar` y las subclases pueden redefinirlo. El polimorfismo permite que, aunque se use una referencia de tipo `Soldado`, se invoque la versión correcta del método según el objeto real.

Esto permite recorrer colecciones heterogéneas de objetos y ejecutar el comportamiento adecuado sin necesidad de usar condicionales explícitos para cada tipo concreto.

Código:
```Java
class Soldado {
    public void saludar() {
        System.out.println("Soy un soldado");
    }
}

class Zapador extends Soldado {
    @Override
    public void saludar() {
        System.out.println("Soy un zapador");
    }
}

class Artillero extends Soldado {
    @Override
    public void saludar() {
        System.out.println("Soy un artillero");
    }
}

class Main {
    public static void main(String[] args) {
        Soldado[] ejercito = {
            new Zapador(),
            new Artillero(),
            new Zapador()
        };

        for (Soldado s : ejercito) {
            s.saludar();
        }
    }
}
```

---

## 4. Si sobreescribo un método, ¿puedo invocar el método base para trabajar a partir de su resultado? Haz que zapador cambie ligeramente la forma de saludar, que salude de forma normal, tal cual hace el soldado base, pero que además añada un "ZAPADOR A SUS ORDENES" ¿qué palabra clave del lenguaje has usado para invocar al método de la clase base?

### Respuesta:

Al sobreescribir un método, es posible invocar la implementación original de la superclase utilizando la palabra clave `super`. Esto permite reutilizar el comportamiento base y añadir funcionalidad adicional sin reescribir todo el método.

Esto es útil cuando se quiere extender ligeramente el comportamiento en lugar de sustituirlo completamente, manteniendo coherencia con la lógica original de la clase base.

Código:
```Java
class Zapador extends Soldado {
    @Override
    public void saludar() {
        super.saludar();
        System.out.println("ZAPADOR A SUS ORDENES");
    }
}
```

---

## 5. Al sobreescribir un método en Java, ¿qué restricciones existen sobre los tipos de los parámetros y el tipo de retorno? ¿Qué diferencia hay entre sobreescritura (*overriding*) y sobrecarga (*overloading*)? ¿Para qué sirve la anotación `@Override` y por qué es recomendable usarla siempre?

### Respuesta:

En la sobreescritura, el método debe tener la misma firma (nombre y parámetros) y el tipo de retorno debe ser compatible (igual o covariante). No se pueden cambiar arbitrariamente los parámetros ni lanzar excepciones más restrictivas que las del método original.

La sobrecarga (overloading) es diferente: consiste en definir varios métodos con el mismo nombre pero distintos parámetros. La anotación `@Override` se usa para indicar que un método está sobrescribiendo otro, y es recomendable porque el compilador puede detectar errores si la firma no coincide exactamente.

---

## 6. Entonces, cuando se estudia Java, ¿se emplea el polimorfismo desde el principio? Por ejemplo, sobreescribiendo `toString` o sobreescribiendo `equals`, ¿ya estoy usando polimorfismo?

### Respuesta


## 7. ¿Qué es una **"clase abstracta"**? ¿Qué es un **"método abstracto"**? ¿Puedo crear instancias de una clase abstracta? Pongamos un ejemplo en Java: Redefinamos `Soldado`, hagamos que, además del método `saluda` que ya tenía, tenga un método `atacar`, que sea abstracto y que cada tipo de soldado haga su acción cuando se le pida atacar. ¿Donde debemos poner `abstract`?

### Respuesta


## 8. ¿Qué efecto tiene la palabra clave `final` sobre métodos y clases en Java? ¿Cómo se relaciona con el polimorfismo? ¿Conoces algún ejemplo de clase `final` en la propia API estándar de Java?

### Respuesta


## 9. En Java, qué son las **"interfaces"**? ¿Son como clases abstractas? ¿Una clase puede implementar más de una interfaz?

### Respuesta


## 10. Vamos a poner un ejemplo nuevo con polimorfismo. Queremos implementar una clase `Punto`, con un método `calcularDistanciaA`, que permite calcular la distancia a otro `Punto`. Sin embargo, como queremos trabajar con puntos 2D y 3D, haz que ese método sea abstracto y haya dos implementaciones de ese cálculo de distancia. Emplea `instanceof` y *downcasting* para verificar que se recibe un punto compatible y poder calcular correctamente la distancia siempre entre puntos del mismo subtipo. Aprovecha este diseño para crear ahora una clase `Linea`, que acepta `Punto`, sin saber de qué tipo es, y es capaz de dar su longitud independientemente de las dimensiones de sus puntos (las cuales desconoce).

### Respuesta


## 11. ¿Qué es la **"herencia de interfaces"** en Java? ¿Existe **"herencia múltiple de interfaces"**? Pon un ejemplo de una interfaz `Fichero` que tenga un método para leer su contenido en forma de `String` y luego dicha interfaz sea extendida por otra que sea `FicheroEscribible` que permita enviar contenido e incluso eliminar el fichero.

### Respuesta
