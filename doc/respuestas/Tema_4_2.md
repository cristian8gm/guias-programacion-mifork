<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Herencia". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación, Excepciones y Composición.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# Tema 4.2. Herencia

## 1. En orientación a objetos, ¿qué es la **herencia** y su relación con "A es-un B"?. Explica las dos implicaciones principales: (1) **compatibilidad de tipos** y (2) **herencia de estado y comportamiento**. Pon un ejemplo en Java muy sencillo, donde un `Soldado` tiene un `nombre` (privado) y un método `saludar()` que muestra su nombre. Hay dos subtipos: un `Artillero`, que es capaz de disparar cohetes y un `Zapador` que pone minas, ambos heredan el atributo nombre y la capacidad de saludar. Además, y de forma específica, el artillero tiene un número de cohetes y el zapador un número de minas, accesibles mediante "getters" específicos. Respecto a la compatibilidad de tipos, aprovechémosla: crea un array de `Soldado`, mete varios de distinto tipo (son todos compatibles con `Soldado`). Recórrela y que todos te saluden.

### Respuesta:

La herencia es un mecanismo de la programación orientada a objetos que permite definir una clase nueva a partir de otra existente. La relación que se establece se suele expresar como “A es-un B”, lo que significa que la subclase representa un caso más concreto del concepto definido por la superclase. Este vínculo no es solo semántico, sino que tiene consecuencias técnicas importantes en el lenguaje.

La primera implicación es la **compatibilidad de tipos**: un objeto de la subclase puede usarse allí donde se espera un objeto de la superclase. La segunda implicación es la **herencia de estado y comportamiento**: la subclase hereda los atributos y métodos accesibles de la superclase, pudiendo reutilizarlos o ampliarlos. Esto permite compartir código y definir jerarquías coherentes de conceptos.

Código:
```Java
class Soldado {
    private String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Soy el soldado " + nombre);
    }
}

class Artillero extends Soldado {
    private int cohetes;

    public Artillero(String nombre, int cohetes) {
        super(nombre);
        this.cohetes = cohetes;
    }

    public int getCohetes() {
        return cohetes;
    }
}

class Zapador extends Soldado {
    private int minas;

    public Zapador(String nombre, int minas) {
        super(nombre);
        this.minas = minas;
    }

    public int getMinas() {
        return minas;
    }
}

class Main {
    public static void main(String[] args) {
        Soldado[] ejercito = new Soldado[3];
        ejercito[0] = new Artillero("Luis", 5);
        ejercito[1] = new Zapador("Carlos", 3);
        ejercito[2] = new Artillero("Ana", 2);

        for (Soldado s : ejercito) {
            s.saludar();
        }
    }
}
```

---

## 2. Al crear los soldados concretos, ¿cuántos constructores se ejecutan y en qué orden? ¿Qué significa `super` dentro de un constructor? Si la clase base no tiene visible el constructor sin parámetros, ¿debo llamar a `super` siempre? 

### Respuesta:

Cuando se crea un objeto de una subclase, se ejecutan siempre los constructores de toda la jerarquía de herencia. Primero se ejecuta el constructor de la superclase y después el de la subclase. Esto garantiza que la parte del objeto correspondiente a la superclase quede correctamente inicializada antes de inicializar la parte específica de la subclase.

La palabra clave `super` dentro de un constructor se utiliza para invocar explícitamente al constructor de la clase base. Si la superclase no dispone de un constructor sin parámetros accesible, es obligatorio llamar a `super(...)` con los argumentos adecuados. De lo contrario, el código no compilará, ya que Java exige que la superclase se inicialice correctamente.

---

## 3. Respecto a los objetos de subclases en memoria, los atributos privados de la superclase, ¿forman parte de una instancia de la subclase en memoria? En caso afirmativo ¿implica que se puedan usar desde el código de la subclase? Explícalo con el ejemplo de `Soldado` y alguna de sus subclases.

### Respuesta:

Los atributos privados definidos en la superclase forman parte de la instancia completa del objeto, incluso cuando dicho objeto pertenece a una subclase. Es decir, un objeto `Artillero` contiene en memoria los atributos de `Soldado`, incluidos los privados, junto con los suyos propios.

Sin embargo, que formen parte del objeto no implica que puedan ser accedidos directamente desde el código de la subclase. El modificador `private` restringe el acceso al propio cuerpo de la clase donde se declara. Por ello, un `Artillero` no puede acceder directamente al atributo `nombre`, aunque este exista físicamente dentro del objeto.

---

## 4. ¿Qué implica en términos de **extensibilidad** de código el hecho de que sean compatibles a nivel de tipos? Ilustra esto añadiendo un nuevo tipo de `Soldado` y demostrando que el código para pedir el saludo a todos los soldados no se modifica.

### Respuesta:

La compatibilidad de tipos permite escribir código que trabaja con la superclase sin conocer las subclases concretas. Esto mejora enormemente la extensibilidad, ya que se pueden añadir nuevos tipos derivados sin modificar el código existente que opera sobre la jerarquía.

Por ejemplo, si se añade un nuevo tipo de `Soldado`, el código que recorre el array y pide a todos los soldados que saluden no necesita cambiar. El nuevo tipo encaja automáticamente en el diseño, cumpliendo el principio de “abierto para extensión, cerrado para modificación”.

Código:
```Java
class Medico extends Soldado {
    public Medico(String nombre) {
        super(nombre);
    }
}
```

---

## 5. En Java, cuando trabajo con referencias y herencia. ¿Puedo tener una referencia del supertipo que apunte a objetos reales de un subtipo? ¿Puedo invocar con la referencia del supertipo a métodos públicos del subtipo? ¿En qué consiste el **"upcasting"** y el **"downcasting"**? ¿Qué es el `instanceof`? Pon un ejemplo de recorrido de un array de `Soldado`, comprobando que, si el objeto real es un `Artillero`, solicite el número de cohetes que tiene y los imprima.

### Respuesta:

En Java, es posible que una referencia del supertipo apunte a un objeto real de un subtipo. Sin embargo, con una referencia del supertipo solo se pueden invocar métodos definidos en dicho supertipo. Para acceder a métodos específicos del subtipo, es necesario realizar un *downcasting* explícito.

El **upcasting** es la conversión automática de una referencia de subtipo a supertipo, mientras que el **downcasting** es la conversión inversa, que debe comprobarse en tiempo de ejecución. El operador `instanceof` permite verificar el tipo real del objeto antes de realizar el casting, evitando errores.

Código:
```Java
for (Soldado s : ejercito) {
    if (s instanceof Artillero) {
        Artillero a = (Artillero) s;
        System.out.println("Cohetes: " + a.getCohetes());
    }
}
```

---

## 6. Respecto a la ocultación de información y herencia, ¿qué significa acceso **"protegido"** de métodos y/o atributos? ¿Cómo se implementa en Java? Pon un ejemplo de uso de en la clase `Soldado` para que su nombre sea protegido y pueda usarse en el método de poner bombas del `Zapador`.

### Respuesta:

El acceso protegido permite que un atributo o método sea accesible desde la propia clase, desde las subclases y desde las clases del mismo paquete. Es una solución intermedia entre `private` y `public`, pensada específicamente para facilitar la herencia sin exponer completamente los detalles internos.

En Java, se implementa con la palabra clave `protected`. De este modo, una subclase puede reutilizar directamente información de la superclase sin romper completamente la encapsulación. Esto es útil cuando las subclases necesitan acceder a ciertos datos heredados para implementar su comportamiento específico.

Código:
```Java
class Soldado {
    protected String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }
}

class Zapador extends Soldado {
    public Zapador(String nombre) {
        super(nombre);
    }

    public void ponerMina() {
        System.out.println(nombre + " coloca una mina");
    }
}
```

---

## 7. En los lenguajes orientados a objetos ¿hay una **clase base** para todos los objetos? ¿Ocurre en todos los lenguajes? ¿Qué ocurre en Java?

### Respuesta:

En muchos lenguajes orientados a objetos existe una clase base común de la que derivan todos los objetos. Esto permite definir comportamientos generales y garantizar un mínimo común para todos los tipos del sistema.

En Java, todas las clases heredan directa o indirectamente de la clase `Object`. Esto significa que todos los objetos tienen métodos como `toString`, `equals` o `hashCode`. No todos los lenguajes siguen este enfoque de forma tan estricta, aunque es bastante habitual.

---

## 8. ¿Qué es la **"herencia múltiple"**? ¿Existe en Java herencia múltiple?

### Respuesta:

La herencia múltiple consiste en permitir que una clase herede de más de una superclase. Esto puede resultar potente, pero también introduce ambigüedades, especialmente cuando varias superclases definen métodos con la misma firma.

Java no permite herencia múltiple de clases. Sin embargo, sí permite implementar múltiples interfaces, lo que ofrece una alternativa segura para compartir contratos de comportamiento sin heredar estado, evitando así muchos de los problemas clásicos de la herencia múltiple.

---

## 9. Las excepciones en los lenguajes orientados a objetos son objetos. Por tanto, se pueden crear excepciones personalizadas. Pon un ejemplo en Java de una excepción personalizada (`UsuarioNoEncontradoException`), que sea *no controlada* y que además este compuesto con un `Usuario`, para saber qué `Usuario` dio el problema. Permite además que se pueda incluir la causa, es decir, sobrecarga el constructor para tener una versión que permita añadir la causa subyacente. 

### Respuesta:

Las excepciones en Java son clases que heredan de `Throwable`. Al crear una excepción personalizada no controlada, se suele heredar de `RuntimeException`. Gracias a la herencia, la nueva excepción se integra plenamente en el sistema de manejo de errores del lenguaje.

Además, al componer la excepción con un objeto de dominio, como `Usuario`, se aporta información contextual valiosa. Al permitir incluir una causa, se conserva la traza del error original, mejorando la depuración.

Código:
```Java
class Usuario {
    private String id;

    public Usuario(String id) {
        this.id = id;
    }

    public String getId() {
        return id;
    }
}

class UsuarioNoEncontradoException extends RuntimeException {
    private Usuario usuario;

    public UsuarioNoEncontradoException(Usuario usuario) {
        this.usuario = usuario;
    }

    public UsuarioNoEncontradoException(Usuario usuario, Throwable causa) {
        super(causa);
        this.usuario = usuario;
    }

    public Usuario getUsuario() {
        return usuario;
    }
}
```

---

## 10. Herencia vs. Composición. Se dice que no se debe emplear herencia simplemente por reutilizar código, es decir, que si quiero reutilizar código simplemente, no debo pensar en herencia como primera opción ¿por qué?

### Respuesta:

No se recomienda usar herencia únicamente para reutilizar código porque la herencia implica una relación conceptual fuerte de tipo “es-un”. Si esta relación no es correcta desde el punto de vista del dominio, el diseño se vuelve frágil y difícil de mantener.

Además, la herencia introduce acoplamiento fuerte entre superclase y subclase. Cambios en la superclase pueden afectar inesperadamente a las subclases, lo que reduce la flexibilidad del sistema y aumenta el riesgo de errores.

---

## 11. Herencia vs. Composición. Se dice que se debe *"favorecer la composición frente a la herencia"*, ¿por qué?

### Respuesta:

Se recomienda favorecer la composición porque permite reutilizar comportamiento sin establecer relaciones rígidas de herencia. Con la composición, los objetos colaboran entre sí, manteniendo responsabilidades bien separadas y reduciendo el acoplamiento.

La composición facilita la sustitución de componentes, el cambio de comportamiento en tiempo de ejecución y una evolución más segura del diseño. Por ello, suele considerarse una alternativa más flexible y robusta que la herencia.

---

## 12. Herencia vs. Composición. Se dice que la *"herencia rompe la encapsulación"*, ¿a qué se refiere esto?

### Respuesta:

Se dice que la herencia rompe la encapsulación porque las subclases dependen de detalles internos de la superclase. Incluso con modificadores como `protected`, las subclases pueden verse afectadas por cambios internos que no deberían ser visibles desde fuera.

Esto provoca que la superclase ya no pueda modificarse libremente sin riesgo de romper a sus subclases. En ese sentido, la herencia expone más de lo deseable, mientras que la composición mantiene mejor aislados los componentes.

---

## 13. Pongamos un ejemplo de dos alternativas para lo mismo. Tenemos un `Estudiante` y un `Trabajador`, ambos tienen datos en común: el DNI y el nombre. Modelemos esto de dos formas: uno por herencia, con una superclase `Persona`, y otro con composición, con una clase `DatosPersonales`. Se debe recibir una instancia de `DatosPersonales` en el constructor de la clase `Estudiante` y `Trabajador`.

### Respuesta:

En el enfoque con herencia, se define una superclase `Persona` que contiene los datos comunes, y las clases `Estudiante` y `Trabajador` heredan de ella. Esto establece una relación “es-un” clara, pero introduce un acoplamiento fuerte entre las clases.

En el enfoque con composición, se encapsulan los datos comunes en una clase `DatosPersonales`. Tanto `Estudiante` como `Trabajador` reciben una instancia de esta clase en su constructor. Este diseño es más flexible y evita jerarquías innecesarias.

Código (herencia):
```Java
class Persona {
    protected String dni;
    protected String nombre;
}

class Estudiante extends Persona { }
class Trabajador extends Persona { }
```

Código (composición):
```Java
class DatosPersonales {
    private String dni;
    private String nombre;

    public DatosPersonales(String dni, String nombre) {
        this.dni = dni;
        this.nombre = nombre;
    }
}

class Estudiante {
    private DatosPersonales datos;

    public Estudiante(DatosPersonales datos) {
        this.datos = datos;
    }
}

class Trabajador {
    private DatosPersonales datos;

    public Trabajador(DatosPersonales datos) {
        this.datos = datos;
    }
}
```