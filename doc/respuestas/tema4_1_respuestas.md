# Tema 4.1. Composición


## 1. En C, podemos crear estructuras mayores **componiendo** unas con otras, que suelen describirse como "A tiene-un/tiene-varios B". Pon un ejemplo, empleando `struct`, de una línea de puntos, donde puntos tienen dos coordenadas (`x` e `y`), y la línea esta hecha de dos puntos. Incluye una función para calcular la distancia entre puntos y otra para hallar la longitud de una línea.

### Respuesta

En C, la composición se logra incluyendo una estructura dentro de otra como campo. No existe orientación a objetos, pero sí se puede modelar claramente la relación “tiene-un”. En este caso, una línea “tiene dos puntos”, y cada punto “tiene” dos coordenadas. Esta forma de composición es puramente estructural y no impone restricciones sobre el ciclo de vida de los elementos.

El lenguaje C permite definir funciones que operan sobre estas estructuras, pasando los datos explícitamente como parámetros. No existe encapsulación real, por lo que los campos pueden ser accedidos libremente desde cualquier parte del programa. Aun así, el ejemplo permite ilustrar claramente la idea conceptual de composición.

El cálculo de la distancia entre puntos se basa en la distancia euclídea en el plano, y la longitud de la línea se obtiene reutilizando dicha función, lo que fomenta la reutilización de código incluso en programación estructurada.

```c
#include <math.h>

typedef struct {
    double x;
    double y;
} Punto;

typedef struct {
    Punto inicio;
    Punto fin;
} Linea;

double distancia(Punto a, Punto b) {
    return sqrt((a.x - b.x) * (a.x - b.x) +
                (a.y - b.y) * (a.y - b.y));
}

double longitudLinea(Linea l) {
    return distancia(l.inicio, l.fin);
}
````

## 2. Ahora transforma ese ejemplo a orientación a objetos con Java, para tener un primer ejemplo de **composición** en orientación a objetos. Crea una clase `Punto`, y una clase `Linea`. La clase `Punto` debe tener un método para calcular distancia a otro `Punto` y `Linea` debe tener un método para calcular su longitud. Gracias a la ocultación de información, supera a C, garantizando que los puntos sean inmutables, al igual que la línea, que una vez creada, no queremos que se modifique de qué a qué puntos va dicha línea.

### Respuesta

En Java, la composición se expresa mediante atributos que son objetos de otras clases. A diferencia de C, se dispone de encapsulación y control de acceso, lo que permite restringir modificaciones externas. Para garantizar la inmutabilidad, se emplean atributos privados y finales, y no se proporcionan métodos mutadores.

La clase `Punto` representa un valor inmutable: una vez creado, sus coordenadas no pueden cambiar. Esto evita efectos colaterales y facilita el razonamiento sobre el código. El cálculo de distancia se encapsula como comportamiento propio del punto, alineándose con los principios de la orientación a objetos.

La clase `Linea` contiene dos puntos también inmutables. Al no permitir modificar sus extremos tras la construcción, se garantiza que la identidad y significado de la línea permanecen constantes durante su ciclo de vida, mejorando la robustez del diseño respecto a la versión en C.

```java
public class Punto {
    private final double x;
    private final double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double distancia(Punto otro) {
        double dx = this.x - otro.x;
        double dy = this.y - otro.y;
        return Math.sqrt(dx * dx + dy * dy);
    }
}

public class Linea {
    private final Punto inicio;
    private final Punto fin;

    public Linea(Punto inicio, Punto fin) {
        this.inicio = inicio;
        this.fin = fin;
    }

    public double longitud() {
        return inicio.distancia(fin);
    }
}
```

## 3. ¿Qué significa la **multiplicidad** en la composición? En el ejemplo anterior, ¿cuál es la multiplicidad entre `Linea` y `Punto`? Indícalo expresando la multiplicidad en ambas direcciones, de `Linea` a `Punto` y de `Punto` a `Linea`.

### Respuesta

La multiplicidad indica cuántas instancias de una clase pueden estar relacionadas con una instancia de otra clase. Es un concepto habitual en diagramas UML y ayuda a precisar las restricciones estructurales de una relación de composición o asociación. No describe comportamiento, sino cardinalidad.

En el ejemplo, desde el punto de vista de `Linea` hacia `Punto`, la multiplicidad es exactamente 2, ya que una línea está compuesta por dos puntos y no por más ni menos. Esta restricción se refleja directamente en el diseño de la clase.

Desde el punto de vista de `Punto` hacia `Linea`, la multiplicidad es de 0 a muchas, ya que un mismo punto puede no pertenecer a ninguna línea o formar parte de varias líneas distintas sin violar ninguna restricción del modelo.

## 4. ¿Qué significa composición **fuerte** y composición **débil**? ¿Qué consecuencia implica en relación al ciclo de vida de los objetos? Indica a cuál solemos referirnos como **"asociación o agregación"** y a cuál como **"composición"** propiamente.

### Respuesta

La composición fuerte implica que el objeto contenido no puede existir sin el contenedor. El ciclo de vida del objeto “parte” está completamente ligado al del objeto “todo”. Cuando el contenedor deja de existir, también lo hacen sus componentes, al menos conceptualmente.

La composición débil, en cambio, no impone esta dependencia de ciclo de vida. El objeto contenido puede existir antes y después del contenedor, y puede incluso ser compartido entre varios contenedores. La relación es más flexible y menos restrictiva.

Habitualmente, la composición débil se denomina asociación o agregación, mientras que el término composición propiamente dicha se reserva para la composición fuerte, donde la dependencia vital entre objetos es clara y explícita.

## 5. Cuando una clase usa a otra al recibirla o devolverla como parámetro en algún método, al hacer `new` dentro de un método, o al usarlas como variables locales, ¿hablamos de composición o de **"dependencia"**?

### Respuesta

En estos casos se habla de dependencia y no de composición. La dependencia describe una relación de uso puntual o temporal, donde una clase necesita conocer a otra para realizar una operación concreta, pero no la mantiene como parte de su estructura interna permanente.

Cuando un objeto se crea dentro de un método o se utiliza como variable local, su alcance y relevancia están limitados al método. No forma parte del estado del objeto que lo usa, por lo que no se puede considerar una relación estructural.

La composición, en cambio, se da cuando una clase mantiene referencias a otras como atributos, formando parte de su identidad y de su estado a lo largo del tiempo.

## 6. En el ejemplo anterior de línea y punto, programa la relación entre `Linea` y `Punto` de dos formas. Una **como composición fuerte**, donde el ciclo de vida de los puntos está ligado al de Linea y otra **como composición débil**, donde no.

### Respuesta

En la composición fuerte, la clase `Linea` crea internamente los puntos y no acepta referencias externas. Conceptualmente, los puntos no existen fuera de la línea y su ciclo de vida está completamente ligado a ella. El cliente no puede reutilizar esos puntos en otro contexto.

En la composición débil, la línea recibe los puntos desde el exterior. Esto permite que los mismos puntos puedan ser compartidos por varias líneas o existir con independencia de ellas. El ciclo de vida de los puntos es autónomo.

Ambas opciones son válidas según el modelo del dominio, pero transmiten significados distintos sobre la relación entre los objetos y deben elegirse con criterio.

```java
// Composición fuerte
public class LineaFuerte {
    private final Punto inicio;
    private final Punto fin;

    public LineaFuerte(double x1, double y1, double x2, double y2) {
        this.inicio = new Punto(x1, y1);
        this.fin = new Punto(x2, y2);
    }
}

// Composición débil
public class LineaDebil {
    private final Punto inicio;
    private final Punto fin;

    public LineaDebil(Punto inicio, Punto fin) {
        this.inicio = inicio;
        this.fin = fin;
    }
}
```

## 7. En Java, en la composición fuerte, ¿cuando el contenedor destruye los objetos? No se observa que `Linea` destruya los `Punto` explícitamente, ¿Por qué?

### Respuesta

En Java, la destrucción de objetos no se realiza de forma explícita, sino que es gestionada automáticamente por el recolector de basura. Un objeto es susceptible de ser destruido cuando deja de existir cualquier referencia alcanzable hacia él.

En una composición fuerte, cuando el contenedor deja de ser accesible, también lo hacen los objetos que contiene, siempre que no existan referencias externas hacia ellos. En ese momento, todos pasan a ser candidatos para la recolección.

Por este motivo no se observa una destrucción explícita de los objetos contenidos. El modelo de memoria de Java abstrae este detalle, permitiendo al programador centrarse en el diseño lógico de las relaciones.

## 8. Pon un ejemplo de composicion débil entre un departamento que tiene varios profesores. Implementa dos composiciones a la vez: entre el departamento y todos sus profesores y entre el departamento y su director, que es un profesor del departamento. Siempre debe haber un director en el departamento desde el inicio. Lanza excepciones si se viola la invariante. Emplea arrays primitivos de Java, estilo `Profesor[]`, con máximo 50, pero no rompas la encapsulación, no desveles que estás empleando un array, permite añadir un `Profesor` al final de la lista, y eliminar un profesor dada su posición. Da acceso a los profesores con un método para saber cuántos hay y otro para obtener un profesor por posición. El director se puede cambiar por otro profesor del departamento. Sin embargo, ten en cuenta esta invariante de clase: el director debe formar siempre parte de la lista de profesores, es decir, ten cuidado al cambiar el director o al eliminar un profesor.

### Respuesta

Este ejemplo representa una composición débil porque los profesores existen independientemente del departamento. El departamento mantiene referencias a ellos, pero no controla su ciclo de vida completo. Se imponen invariantes para garantizar la coherencia interna del estado del departamento.

El uso de arrays primitivos obliga a gestionar manualmente la capacidad y el número de elementos, pero se oculta completamente este detalle mediante métodos públicos. Así se mantiene la encapsulación y se evita acoplar a los clientes con la implementación interna.

Las excepciones se emplean para proteger las invariantes del dominio, como garantizar que siempre exista un director y que el director pertenezca al conjunto de profesores.

```java
public class Profesor {
    private final String nombre;

    public Profesor(String nombre) {
        this.nombre = nombre;
    }
}

public class Departamento {
    private final Profesor[] profesores = new Profesor[50];
    private int numProfesores = 0;
    private Profesor director;

    public Departamento(Profesor directorInicial) {
        if (directorInicial == null) {
            throw new IllegalArgumentException("Debe existir un director");
        }
        this.director = directorInicial;
        profesores[numProfesores++] = directorInicial;
    }

    public void agregarProfesor(Profesor p) {
        if (numProfesores >= 50) {
            throw new IllegalStateException("Capacidad máxima alcanzada");
        }
        profesores[numProfesores++] = p;
    }

    public void eliminarProfesor(int pos) {
        if (pos < 0 || pos >= numProfesores) {
            throw new IndexOutOfBoundsException();
        }
        if (profesores[pos] == director) {
            throw new IllegalStateException("No se puede eliminar al director");
        }
        for (int i = pos; i < numProfesores - 1; i++) {
            profesores[i] = profesores[i + 1];
        }
        numProfesores--;
    }

    public int getNumProfesores() {
        return numProfesores;
    }

    public Profesor getProfesor(int pos) {
        if (pos < 0 || pos >= numProfesores) {
            throw new IndexOutOfBoundsException();
        }
        return profesores[pos];
    }

    public void cambiarDirector(Profesor nuevoDirector) {
        boolean existe = false;
        for (int i = 0; i < numProfesores; i++) {
            if (profesores[i] == nuevoDirector) {
                existe = true;
                break;
            }
        }
        if (!existe) {
            throw new IllegalArgumentException("El director debe ser profesor del departamento");
        }
        this.director = nuevoDirector;
    }
}
```

## 9. En Java, existen también `List`, cambia y muestra cómo sería el código anterior empleando `List` en vez de arrays primitivos. ¿Qué parte del código original te has ahorrado? Además, fíjate en el método `getProfesor(int pos)`: si en su lugar existiera un método que devolviera todos los profesores a la vez, ¿qué problema tendría devolver directamente la lista interna? ¿Cómo lo resolverías?

### Respuesta

El uso de `List` simplifica notablemente el código, ya que se delega en la biblioteca estándar la gestión del tamaño, los desplazamientos al eliminar elementos y las comprobaciones básicas de rango. Esto reduce código repetitivo y potenciales errores.

Se elimina la necesidad de controlar manualmente el contador de elementos y de mover posiciones al borrar. El código resulta más legible, más mantenible y expresa mejor la intención del programador.

Si se devolviera directamente la lista interna, se rompería la encapsulación, ya que el cliente podría modificarla sin control. Para evitarlo, se puede devolver una copia defensiva o una vista no modificable.

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public class Departamento {
    private final List<Profesor> profesores = new ArrayList<>();
    private Profesor director;

    public Departamento(Profesor directorInicial) {
        if (directorInicial == null) {
            throw new IllegalArgumentException();
        }
        this.director = directorInicial;
        profesores.add(directorInicial);
    }

    public void agregarProfesor(Profesor p) {
        profesores.add(p);
    }

    public void eliminarProfesor(int pos) {
        Profesor p = profesores.get(pos);
        if (p == director) {
            throw new IllegalStateException();
        }
        profesores.remove(pos);
    }

    public int getNumProfesores() {
        return profesores.size();
    }

    public Profesor getProfesor(int pos) {
        return profesores.get(pos);
    }

    public List<Profesor> getProfesores() {
        return Collections.unmodifiableList(profesores);
    }
}
```

## 10. Al igual que ocurre con las excepciones en Java, que pueden encerrar causas (que son excepciones), de forma recursiva, suponen un tipo especial de composiciones, denominadas composiciones recursivas. Pon un ejemplo en Java de una `Persona`, que sea inmutable, y que tiene una madre, que es otra `Persona`. Haz un main con un ejemplo de uso con una familia de personas, desde el nieto hasta la abuela. Enumera algún otro ejemplo clásico de composiciones recursivas.

### Respuesta

La composición recursiva se produce cuando una clase contiene una referencia a otra instancia de la misma clase. Esto permite modelar estructuras jerárquicas o encadenadas de forma natural, como ocurre con excepciones, árboles o listas enlazadas.

En este ejemplo, una persona tiene una madre, que a su vez es otra persona. La inmutabilidad garantiza que las relaciones familiares no se alteran tras la creación del objeto, reforzando la coherencia del modelo.

Otros ejemplos clásicos de composición recursiva incluyen los nodos de un árbol, las carpetas que contienen subcarpetas o los elementos de una estructura enlazada.

```java
public class Persona {
    private final String nombre;
    private final Persona madre;

    public Persona(String nombre, Persona madre) {
        this.nombre = nombre;
        this.madre = madre;
    }

    public String getNombre() {
        return nombre;
    }

    public Persona getMadre() {
        return madre;
    }
}

public class Main {
    public static void main(String[] args) {
        Persona abuela = new Persona("Abuela", null);
        Persona madre = new Persona("Madre", abuela);
        Persona hijo = new Persona("Hijo", madre);
    }
}
```

## 11. ¿Qué son las relaciones de composición "bidireccionales"? ¿Qué habría que hacer para implementar este tipo de relación en el ejemplo de `Profesor` y `Departamento`?

### Respuesta

Una composición bidireccional es aquella en la que ambas partes de la relación se conocen mutuamente. Es decir, el contenedor conoce a sus componentes y cada componente mantiene una referencia al contenedor al que pertenece.

Para implementarla en el ejemplo, cada `Profesor` debería tener una referencia a su `Departamento`, además de que el departamento mantenga la lista de profesores. Esto permite navegar la relación en ambos sentidos.

Es necesario extremar el cuidado para mantener la coherencia, actualizando ambas referencias de forma sincronizada y controlada, normalmente encapsulando dichas modificaciones en métodos bien definidos.

```
```
