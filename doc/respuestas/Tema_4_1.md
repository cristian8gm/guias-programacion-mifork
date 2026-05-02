<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Composición". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación y Excepciones.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# Tema 4.1. Composición


## 1. En C, podemos crear estructuras mayores **componiendo** unas con otras, que suelen describirse como "A tiene-un/tiene-varios B". Pon un ejemplo, empleando `struct`, de una línea de puntos, donde puntos tienen dos coordenadas (`x` e `y`), y la línea esta hecha de dos puntos. Incluye una función para calcular la distancia entre puntos y otra para hallar la longitud de una línea.

### Respuesta:

En C, la composición se realiza incluyendo estructuras dentro de otras, siguiendo el patrón “A tiene-un B”. De esta forma, se pueden organizar datos más complejos sin necesidad de programación orientada a objetos. El lenguaje únicamente agrupa memoria, por lo que no existe control sobre la inmutabilidad ni encapsulación; los programadores deben gestionar esos detalles manualmente. Así, una línea puede representarse como dos puntos contenidos directamente dentro del `struct`.

En este ejemplo, se implementa un `struct Punto`, un `struct Linea`, y se proveen funciones auxiliares para calcular la distancia entre puntos y la longitud de una línea. La composición aquí es estrictamente estructural: los puntos son parte de la línea, sin ninguna lógica adicional que garantice integridad o consistencia más allá de la proporcionada por el programad

Código:
```C
#include <math.h>

typedef struct {
    double x;
    double y;
} Punto;

typedef struct {
    Punto a;
    Punto b;
} Linea;

double distancia(Punto p1, Punto p2) {
    double dx = p1.x - p2.x;
    double dy = p1.y - p2.y;
    return sqrt(dx*dx + dy*dy);
}

double longitudLinea(Linea l) {
    return distancia(l.a, l.b);
}
```

---

## 2. Ahora transforma ese ejemplo a orientación a objetos con Java, para tener un primer ejemplo de **composición** en orientación a objetos. Crea una clase `Punto`, y una clase `Linea`. La clase `Punto` debe tener un método para calcular distancia a otro `Punto` y `Linea` debe tener un método para calcular su longitud. Gracias a la ocultación de información, supera a C, garantizando que los puntos sean inmutables, al igual que la línea, que una vez creada, no queremos que se modifique de qué a qué puntos va dicha línea.  

### Respuesta:

En Java, la composición se implementa definiendo clases que contienen referencias a otras clases, con la ventaja de poder aplicar encapsulación e inmutabilidad. La clase `Punto` se declara inmutable usando campos privados y finales, y ofreciendo únicamente métodos de consulta. Esto impide que un punto cambie tras su creación, lo que facilita razonar sobre el estado del sistema.

La clase `Linea` también se diseña como inmutable, con dos campos tipo `Punto` que se establecen en el constructor y no pueden cambiar. Así se garantiza que una vez creada una línea, sus extremos permanecen fijos. Java permite que las relaciones entre clases expresen significados del dominio de forma clara y segura, superando las limitaciones de C.

Código:
```Java
final class Punto {
    private final double x;
    private final double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double distancia(Punto otro) {
        return Math.hypot(this.x - otro.x, this.y - otro.y);
    }
}

final class Linea {
    private final Punto a;
    private final Punto b;

    public Linea(Punto a, Punto b) {
        this.a = a;
        this.b = b;
    }

    public double longitud() {
        return a.distancia(b);
    }
}
```

---

## 3. ¿Qué significa la **multiplicidad** en la composición? En el ejemplo anterior, ¿cuál es la multiplicidad entre `Linea` y `Punto`? Indícalo expresando la multiplicidad en ambas direcciones, de `Linea` a `Punto` y de `Punto` a `Linea`.

### Respuesta:

La multiplicidad expresa cuántos objetos de un tipo están asociados, contenidos o compuestos por un objeto de otro tipo. Es un concepto habitual en UML y en diseño orientado a objetos, y permite describir relaciones como “uno a uno”, “uno a muchos” o “muchos a muchos”. Esta idea ayuda a comprender de forma precisa la estructura y cardinalidad entre clases.

En el ejemplo, una `Linea` tiene exactamente dos `Punto`, por lo que la multiplicidad de `Linea → Punto` es **1..1 para cada uno de los dos puntos**, o bien puede representarse como “2”. Por el contrario, un `Punto` puede pertenecer a cero o muchas líneas distintas, según el diseño del programa, por lo que la multiplicidad `Punto → Linea` sería **0..***.

---

## 4. ¿Qué significa composición **fuerte** y composición **débil**? ¿Qué consecuencia implica en relación al ciclo de vida de los objetos? Indica a cuál solemos referirnos como **"asociación o agregación"** y a cuál como **"composición"** propiamente.

### Respuesta:

La composición fuerte implica una relación donde el objeto “todo” controla completamente el ciclo de vida de sus partes. Si el objeto contenedor deja de existir, también desaparecen las partes. Esto corresponde al concepto de *composición* en UML. El vínculo es rígido, y las partes no tienen sentido fuera del todo, reflejando un alto grado de cohesión.

La composición débil (o agregación) representa una relación más flexible donde los objetos pueden existir de manera independiente. El contenedor solo mantiene referencias, pero no es responsable de destruir sus componentes. Esta forma de relación se denomina *agregación* en UML y es útil para representar asociaciones como departamento-profesor o coche-conductor.

---

## 5. Cuando una clase usa a otra al recibirla o devolverla como parámetro en algún método, al hacer `new` dentro de un método, o al usarlas como variables locales, ¿hablamos de composición o de **"dependencia"**?

### Respuesta:

Cuando una clase utiliza otra únicamente como parámetro, valor de retorno o variable local dentro de un método, se habla de **dependencia**, no de composición. Esta relación implica que una clase depende funcionalmente de otra para realizar tareas específicas, pero no la “posee” como parte permanente de su estructura interna.

En este tipo de relación, los objetos no forman parte del estado del objeto actual, sino que se usan temporalmente para completar una operación. Por ello, la dependencia es una relación más débil y efímera que la composición, y no forma parte del ciclo de vida del objeto que depende.

---

## 6. En el ejemplo anterior de línea y punto, programa la relación entre `Linea` y `Punto` de dos formas. Una **como composición fuerte**, donde el ciclo de vida de los puntos está ligado al de Linea y otra **como composición débil**, donde no.

### Respuesta:

En composición fuerte, la `Linea` crea internamente sus puntos. Esto vincula completamente el ciclo de vida de los puntos al de la línea. Si la línea desaparece, no queda ninguna forma de acceder a los puntos, pues no existen fuera del objeto que los contiene.

En composición débil, la `Linea` recibe puntos externos y solo mantiene referencias. Los puntos pueden existir antes y después de la línea, y pueden ser compartidos. Esto aporta flexibilidad y evita la duplicación de entidades en un sistema más amplio.

Composición fuerte:
```Java
class LineaFuerte {
    private final Punto a = new Punto(0, 0);
    private final Punto b = new Punto(1, 1);
}
```

Composición débil:
```Java
class LineaDebil {
    private final Punto a;
    private final Punto b;

    public LineaDebil(Punto a, Punto b) {
        this.a = a;
        this.b = b;
    }
}
```

---

## 7. En Java, en la composición fuerte, ¿cuando el contenedor destruye los objetos? No se observa que `Linea` destruya los `Punto` explícitamente, ¿Por qué?

### Respuesta:

En Java, la destrucción explícita no existe; el programador no borra objetos manualmente. La eliminación de un objeto ocurre automáticamente mediante el recolector de basura (garbage collector), cuando ya no hay referencias accesibles a dicho objeto. Este proceso es transparente y no predecible en cuanto al momento exacto de ejecución.

Por esta razón, aunque `Linea` contenga puntos en composición fuerte, no los “destruye” directamente. Basta con que el objeto `Linea` deje de ser accesible. En ese momento, los puntos también quedan inalcanzables y serán recogidos por el recolector de basura de manera natural.

---

## 8. Pon un ejemplo de composicion débil entre un departamento que tiene varios profesores. Implementa dos composiciones a la vez: entre el departamento y todos sus profesores y entre el departamento y su director, que es un profesor del departamento. Siempre debe haber un director en el departamento desde el inicio. Lanza excepciones si se viola la invariante. Emplea arrays primitivos de Java, estilo `Profesor[]`, con máximo 50, pero no rompas la encapsulación, no desveles que estás empleando un array, permite añadir un `Profesor` al final de la lista, y eliminar un profesor dada su posición. Da acceso a los profesores con un método para saber cuántos hay y otro para obtener un profesor por posición. El director se puede cambiar por otro profesor del departamento. Sin embargo, ten en cuenta esta invariante de clase: el director debe formar siempre parte de la lista de profesores, es decir, ten cuidado al cambiar el director o al eliminar un profesor.

### Respuesta:

Aquí se implementa un departamento que tiene hasta 50 profesores. La composición es débil porque los profesores pueden existir fuera del departamento. Se incluye además un director, que debe ser uno de los profesores del departamento, constituyendo una invariante que se preserva mediante métodos que lanzan excepciones si se viola.

Para evitar exponer la representación interna basada en un array, se ofrecen métodos de acceso controlados: un getter del número de profesores y otro para obtener uno por posición. También se incluyen métodos para añadir y eliminar profesores y para cambiar el director validando que siempre pertenezca al departamento.

Código:
```Java	
class Profesor {
    private final String nombre;

    public Profesor(String nombre) {
        this.nombre = nombre;
    }
}

class Departamento {
    private final Profesor[] profesores = new Profesor[50];
    private int numProf = 0;
    private Profesor director;

    public Departamento(Profesor directorInicial) {
        profesores[0] = directorInicial;
        numProf = 1;
        director = directorInicial;
    }

    public void addProfesor(Profesor p) {
        if (numProf >= 50) throw new IllegalStateException("Máximo alcanzado");
        profesores[numProf++] = p;
    }

    public void removeProfesor(int pos) {
        if (pos < 0 || pos >= numProf) throw new IllegalArgumentException();
        if (profesores[pos] == director)
            throw new IllegalStateException("No se puede eliminar al director");
        for (int i = pos; i < numProf - 1; i++) {
            profesores[i] = profesores[i + 1];
        }
        profesores[--numProf] = null;
    }

    public int getNumProfesores() {
        return numProf;
    }

    public Profesor getProfesor(int pos) {
        if (pos < 0 || pos >= numProf) throw new IllegalArgumentException();
        return profesores[pos];
    }

    public void cambiarDirector(Profesor nuevo) {
        boolean encontrado = false;
        for (int i = 0; i < numProf; i++) {
            if (profesores[i] == nuevo) {
                encontrado = true;
                break;
            }
        }
        if (!encontrado)
            throw new IllegalArgumentException("El director debe estar en el departamento");
        director = nuevo;
    }
}
```

---

## 9. En Java, existen también `List`, cambia y muestra cómo sería el código anterior empleando `List` en vez de arrays primitivos. ¿Qué parte del código original te has ahorrado? Además, fíjate en el método `getProfesor(int pos)`: si en su lugar existiera un método que devolviera todos los profesores a la vez, ¿qué problema tendría devolver directamente la lista interna? ¿Cómo lo resolverías?

### Respuesta:

Al emplear `List`, desaparece la necesidad de manejar manualmente índices, huecos y desplazamientos dentro del array. Métodos como `add`, `remove` y `size` simplifican notablemente la gestión de la colección, reduciendo la posibilidad de errores y mejorando la claridad del código.

Sin embargo, devolver directamente la lista interna expondría la representación y permitiría que código externo modifique la colección sin control. Para evitarlo, se puede devolver una copia inmutable o una vista no modificable, lo que mantiene la encapsulación y garantiza la integridad del objeto.

Código simplificado:
```Java
import java.util.*;

class Departamento {
    private final List<Profesor> profesores = new ArrayList<>();
    private Profesor director;

    public Departamento(Profesor directorInicial) {
        profesores.add(directorInicial);
        director = directorInicial;
    }

    public void addProfesor(Profesor p) {
        profesores.add(p);
    }

    public void removeProfesor(int pos) {
        Profesor p = profesores.get(pos);
        if (p == director)
            throw new IllegalStateException("No se puede eliminar al director");
        profesores.remove(pos);
    }
    public int getNumProfesores() {
        return profesores.size();
    }

    public Profesor getProfesor(int pos) {
        return profesores.get(pos);
    }

    public List<Profesor> getProfesoresCopia() {
        return Collections.unmodifiableList(new ArrayList<>(profesores));
    }
}
```

---

## 10. Al igual que ocurre con las excepciones en Java, que pueden encerrar causas (que son excepciones), de forma recursiva, suponen un tipo especial de composiciones, denominadas composiciones recursivas. Pon un ejemplo en Java de una `Persona`, que sea inmutable, y que tiene una madre, que es otra `Persona`. Haz un main con un ejemplo de uso con una familia de personas, desde el nieto hasta la abuela. Enumera algún otro ejemplo clásico de composiciones recursivas.

### Respuesta:

La composición recursiva ocurre cuando una clase contiene una instancia de sí misma. Esto permite representar estructuras como árboles genealógicos, árboles sintácticos, nodos de directorio, etc. Si además se exige inmutabilidad, se garantiza que los datos genealógicos no cambian una vez creados.

Aquí se implementa una clase `Persona` inmutable con un atributo `madre`, que es otra `Persona`. Se muestra también un ejemplo de uso encadenando varias generaciones. Las composiciones recursivas aparecen frecuentemente en estructuras jerárquicas como carpetas, expresiones matemáticas o listas enlazadas.

Código:
```Java
final class Persona {
    private final String nombre;
    private final Persona madre;

    public Persona(String nombre, Persona madre) {
        this.nombre = nombre;
        this.madre = madre;
    }
}

class Main {
    public static void main(String[] args) {
        Persona abuela = new Persona("Ana", null);
        Persona madre = new Persona("Beatriz", abuela);
        Persona nieto = new Persona("Carlos", madre);
    }
}
```

---

## 11. ¿Qué son las relaciones de composición "bidireccionales"? ¿Qué habría que hacer para implementar este tipo de relación en el ejemplo de `Profesor` y `Departamento`?

### Respuesta:

Una composición bidireccional es aquella en la que dos clases mantienen referencias mutuamente. Esto implica que cada objeto conoce al otro, y ambos representan la relación desde su propio punto de vista. Este tipo de diseño requiere especial cuidado para garantizar consistencia, evitando duplicación o desincronización.

En el caso de `Profesor` y `Departamento`, implicaría que el departamento mantiene una lista de profesores, y cada profesor mantiene una referencia a su departamento. Esta relación debe actualizarse en ambos sentidos cada vez que se añada o elimine un profesor, lo que exige métodos bien diseñados para preservar las invariantes y evitar estados inconsistentes.