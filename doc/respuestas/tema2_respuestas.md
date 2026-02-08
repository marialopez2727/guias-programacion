# **TEMA 2. Encapsulación**

---

## **1. En POO, ¿qué buscan la encapsulación y la ocultación de información? Ventajas.**

La encapsulación busca agrupar datos y operaciones relacionadas dentro de una misma unidad lógica, normalmente una clase. La idea es que el objeto controle cómo se accede y modifica su estado interno, evitando que otras partes del programa manipulen directamente sus datos. Esto permite que el comportamiento del objeto sea coherente y predecible.

La ocultación de información complementa la encapsulación restringiendo el acceso a los detalles internos. Al ocultar los atributos y exponer solo lo necesario, se reduce la posibilidad de errores provocados por un uso indebido del estado interno. Entre sus ventajas destacan la reducción del acoplamiento, la facilidad para modificar la implementación sin afectar a otros módulos y la protección de las invariantes de clase.

---

## **2. ¿Qué es la interfaz pública de un objeto o clase? Relación con la ocultación.**

La interfaz pública de una clase es el conjunto de métodos accesibles desde fuera de ella. Representa aquello que otros objetos pueden utilizar para interactuar con la instancia, sin necesidad de conocer cómo está implementada internamente. Es, en esencia, la “cara visible” del objeto.

La ocultación de información se basa precisamente en separar la interfaz pública de la implementación interna. Al mantener los atributos privados y exponer solo métodos controlados, se garantiza que el uso externo del objeto se realice de forma segura y coherente. Esto permite modificar la implementación sin afectar a quienes utilizan la clase.

---

## **3. ¿Por qué diseñar con cuidado la interfaz pública? ¿Es fácil cambiarla?**

La interfaz pública debe diseñarse con cuidado porque constituye un contrato con el resto del programa. Una vez que otros módulos dependen de ella, cualquier cambio puede provocar errores o requerir modificaciones extensas en el código que la utiliza. Por ello, una interfaz mal diseñada puede generar rigidez y dificultades de mantenimiento.

Cambiar la interfaz pública no suele ser sencillo, especialmente en sistemas grandes. Aunque la implementación interna pueda modificarse libremente, la interfaz pública debe mantenerse estable para no romper compatibilidad. Por eso se recomienda exponer solo lo necesario y evitar comprometerse con detalles que podrían cambiar en el futuro.

---

## **4. ¿Qué son las invariantes de clase y por qué ayuda la ocultación?**

Las invariantes de clase son condiciones que deben cumplirse siempre para que un objeto sea válido. Por ejemplo, que un valor no sea negativo o que dos atributos mantengan una relación coherente. Estas condiciones deben mantenerse antes y después de cada operación pública del objeto.

La ocultación de información ayuda porque impide que el estado interno sea modificado de forma arbitraria desde fuera. Al controlar el acceso mediante métodos, la clase puede verificar y garantizar que las invariantes se respeten. Esto reduce errores y asegura que el objeto se mantenga en un estado consistente.

---

## **5. Clase `Punto` con ocultación. Interfaz pública. Significado de `public` y `private`.**

```java
public class Punto {
    private double x;
    private double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double calcularDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }
}
```

La interfaz pública de esta clase está formada por el constructor y el método `calcularDistanciaAOrigen`. Son los elementos accesibles desde fuera y que permiten crear y utilizar objetos `Punto`. Los atributos `x` e `y` no forman parte de la interfaz pública porque están declarados como privados.

En Java, `public` indica que un miembro es accesible desde cualquier parte del programa. Por el contrario, `private` restringe el acceso únicamente a la propia clase. Esto permite controlar cómo se manipulan los datos internos y aplicar la ocultación de información.

---

## **6. ¿A quiénes se pueden aplicar `public` o `private` en Java?**

En Java, los modificadores `public` y `private` pueden aplicarse tanto a clases (aunque solo a clases de nivel superior `public`) como a miembros de clase, es decir, atributos, métodos y constructores. Esto permite controlar la visibilidad de cada elemento de forma granular.

Los atributos y métodos suelen ser los principales candidatos para estos modificadores, ya que determinan qué partes de la clase son accesibles desde fuera. También pueden aplicarse a constructores para controlar cómo se crean los objetos, lo cual es útil en patrones como factorías o singletons.

---

## **7. ¿Existen más tipos de visibilidad? ¿Qué ocurre en Java y en otros lenguajes?**

Además de `public` y `private`, Java ofrece otros niveles de visibilidad. Uno de ellos es `protected`, que permite el acceso desde la propia clase, sus subclases y otras clases del mismo paquete. También existe la visibilidad por defecto (package-private), que se aplica cuando no se especifica ningún modificador y permite el acceso solo dentro del mismo paquete.

Otros lenguajes orientados a objetos pueden ofrecer más niveles o variaciones. Por ejemplo, C++ distingue entre `public`, `protected` y `private`, pero su semántica difiere ligeramente. Algunos lenguajes modernos incluyen visibilidades más específicas, como `internal` o `friend`, para controlar aún más el acceso.

---

## **8. ¿Los miembros privados están ocultos para otras clases o para otras instancias?**

Los miembros privados están ocultos para otras clases, pero no para otras instancias de la misma clase. Esto significa que un objeto puede acceder a los atributos privados de otro objeto si ambos pertenecen a la misma clase. La restricción se aplica a nivel de clase, no de instancia.

```java
public double calcularDistanciaAPunto(Punto otro) {
    double dx = this.x - otro.x;
    double dy = this.y - otro.y;
    return Math.sqrt(dx * dx + dy * dy);
}
```

En este ejemplo, aunque `x` e `y` son privados, el método puede acceder a `otro.x` y `otro.y` porque `otro` es un objeto de la misma clase `Punto`. Esto permite implementar operaciones entre objetos sin romper la encapsulación.

---

## **9. ¿Qué son los métodos getter y setter?**

Los métodos getter y setter son funciones públicas que permiten acceder y modificar atributos privados de una clase. Un getter devuelve el valor de un atributo, mientras que un setter permite asignarle un nuevo valor. Son una forma controlada de exponer el estado interno sin hacerlo público directamente.

Su uso permite validar datos antes de modificarlos, mantener invariantes y registrar cambios si es necesario. Aunque parecen equivalentes a hacer públicos los atributos, ofrecen un punto de control que evita inconsistencias y errores.

---

## **10. ¿La “seguridad” de la ocultación significa evitar hackeos?**

Cuando se dice que la ocultación de información mejora la “seguridad”, no se refiere a seguridad informática en el sentido de evitar ataques o hackeos. Se refiere a la seguridad del diseño del software, es decir, a evitar que el estado interno de los objetos sea manipulado de forma incorrecta.

La ocultación protege la coherencia del programa y reduce errores lógicos. Aunque contribuye indirectamente a un código más robusto, no sustituye mecanismos de seguridad como cifrado, autenticación o control de accesos.

---

## **11. Diferencia entre miembro de instancia y miembro de clase. ¿Se pueden ocultar?**

Un miembro de instancia pertenece a cada objeto individual; cada instancia tiene su propia copia. En cambio, un miembro de clase pertenece a la clase en sí y es compartido por todas las instancias. En Java, los miembros de clase se declaran con la palabra clave `static`.

Ambos tipos de miembros pueden ocultarse utilizando `private`. Esto permite controlar tanto el estado individual de cada objeto como el estado compartido entre todos ellos, manteniendo la encapsulación en ambos niveles.

---

## **12. ¿Tiene sentido que los constructores sean privados?**

En algunos casos sí tiene sentido que los constructores sean privados. Esto ocurre cuando se quiere controlar estrictamente cómo se crean los objetos. Por ejemplo, en el patrón Singleton, el constructor es privado para evitar que se creen múltiples instancias.

También es útil cuando se desea obligar al uso de métodos factoría estáticos, que pueden aplicar validaciones, transformaciones o reutilizar instancias existentes. De esta forma, la clase controla completamente su proceso de creación.

---

## **13. Miembros de clase en Java. Ejemplo en `Punto`.**

Los miembros de clase se indican con la palabra clave `static`. Estos atributos o métodos pertenecen a la clase y no a las instancias. En la clase `Punto`, se puede añadir un seguimiento de los valores máximos de `x` e `y` establecidos:

```java
public class Punto {
    private double x;
    private double y;

    private static double maxX = Double.NEGATIVE_INFINITY;
    private static double maxY = Double.NEGATIVE_INFINITY;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
        if (x > maxX) maxX = x;
        if (y > maxY) maxY = y;
    }

    public static double getMaxX() { return maxX; }
    public static double getMaxY() { return maxY; }
}
```

Estos atributos estáticos registran información global sobre todas las instancias creadas, y su visibilidad puede controlarse igual que la de cualquier otro miembro.

---

## **14. Método factoría que redondea coordenadas.**

```java
public static Punto crearRedondeado(double x, double y) {
    return new Punto(Math.round(x), Math.round(y));
}
```

Se utiliza `static` porque el método pertenece a la clase y no a una instancia concreta. Este método permite crear puntos aplicando una transformación previa a los valores.

---

## **15. Implementación de `Punto` usando un array interno.**

```java
public class Punto {
    private double[] coords = new double[2];

    public Punto(double x, double y) {
        coords[0] = x;
        coords[1] = y;
    }

    public double calcularDistanciaAOrigen() {
        return Math.sqrt(coords[0] * coords[0] + coords[1] * coords[1]);
    }
}
```

La interfaz pública no cambia, pero la implementación interna sí. Esto demuestra cómo la ocultación permite modificar la estructura interna sin afectar a quienes usan la clase.

---

## **16. Si hay getter y setter, ¿por qué no declarar el atributo público?**

Aunque un atributo tenga getter y setter, declararlo público elimina cualquier posibilidad de control. Con un setter se pueden validar valores, mantener invariantes o registrar cambios. Si el atributo es público, cualquier parte del programa podría modificarlo sin restricciones.

La convención habitual es declarar todos los atributos como privados. Esto está directamente relacionado con las invariantes de clase, ya que permite garantizar que el estado del objeto siempre sea válido.

---

## **17. ¿Qué es una clase inmutable? ¿Qué es un método modificador?**

Una clase inmutable es aquella cuyo estado no puede cambiar después de ser creada. Todos sus atributos son privados y no existen métodos que los modifiquen. En lugar de cambiar el estado, los métodos devuelven nuevas instancias con los cambios aplicados.

Un método modificador es aquel que altera el estado interno del objeto. No todos los métodos modificadores son setters; un método puede modificar varios atributos a la vez sin ser un setter tradicional. Las clases inmutables tienen ventajas como simplicidad, seguridad frente a errores y facilidad para trabajar en entornos concurrentes.

---

## **18. ¿Es recomendable incluir setters siempre?**

No es recomendable incluir setters por defecto. Solo deben añadirse cuando realmente sea necesario permitir la modificación del estado. En muchos casos, los objetos deben ser inmutables o tener un estado controlado que no pueda cambiar libremente.

Incluir setters indiscriminadamente debilita la encapsulación y puede romper invariantes. Por ello, se recomienda diseñar cuidadosamente qué partes del estado deben ser modificables y cuáles no.

---

## **19. ¿`String` es mutable o inmutable? Concatenación.**

La clase `String` en Java es inmutable. Cada vez que se concatena una cadena, no se modifica la original, sino que se crea un nuevo objeto con el resultado. Esto puede ser costoso si se realizan muchas concatenaciones en un bucle.

Para operaciones intensivas de concatenación, se recomienda usar `StringBuilder` o `StringBuffer`, que son clases mutables diseñadas para construir cadenas de forma eficiente.

---

## **20. Comparación de objetos. Método `equals`. Cadenas.**

En POO, los objetos pueden compararse por identidad (si son el mismo objeto) o por contenido (si representan la misma información). En Java, el operador `==` compara identidad, mientras que el método `equals` compara contenido, siempre que esté correctamente sobrescrito.

Por defecto, `equals` en la clase `Object` compara identidad, por lo que muchas clases deben sobrescribirlo para comparar contenido. En el caso de las cadenas, deben compararse con `equals`, ya que `==` solo indica si son la misma instancia.

---

## **21. ¿Qué son las clases wrapper? ¿Ventajas?**

Las clases wrapper son clases que encapsulan tipos primitivos para tratarlos como objetos. En Java existen `Integer`, `Double`, `Boolean`, entre otros. El proceso de conversión entre primitivo y wrapper puede hacerse automáticamente mediante autoboxing y unboxing.

Estas clases permiten almacenar valores primitivos en colecciones genéricas, utilizar métodos asociados y representar valores nulos. No todos los lenguajes necesitan wrappers, ya que algunos no distinguen entre tipos primitivos y objetos.

---

## **22. ¿Qué es un tipo enumerado? ¿Ventajas en Java?**

Un tipo enumerado representa un conjunto fijo de valores posibles. En Java, los enumerados son clases especiales que permiten definir constantes con comportamiento y atributos propios. Esto los hace más potentes que los enumerados tradicionales de otros lenguajes.

En términos de encapsulación, los enumerados permiten ocultar detalles internos y exponer solo lo necesario. Además, garantizan que solo existan las instancias definidas, evitando errores y proporcionando seguridad en el diseño.

---

## **23. Enumerado `Mes` con métodos y atributos privados.**

```java
public enum Mes {
    ENERO(31, 1), FEBRERO(28, 2), MARZO(31, 3),
    ABRIL(30, 4), MAYO(31, 5), JUNIO(30, 6),
    JULIO(31, 7), AGOSTO(31, 8), SEPTIEMBRE(30, 9),
    OCTUBRE(31, 10), NOVIEMBRE(30, 11), DICIEMBRE(31, 12);

    private int dias;
    private int ordinal;

    Mes(int dias, int ordinal) {
        this.dias = dias;
        this.ordinal = ordinal;
    }

    public int getDias() { return dias; }
    public int getOrdinal() { return ordinal; }

    public boolean esDeInvierno(boolean norte) {
        return norte ? (this == DICIEMBRE || this == ENERO || this == FEBRERO)
                     : (this == JUNIO || this == JULIO || this == AGOSTO);
    }

    public boolean esDePrimavera(boolean norte) {
        return norte ? (this == MARZO || this == ABRIL || this == MAYO)
                     : (this == SEPTIEMBRE || this == OCTUBRE || this == NOVIEMBRE);
    }

    public boolean esDeVerano(boolean norte) {
        return norte ? (this == JUNIO || this == JULIO || this == AGOSTO)
                     : (this == DICIEMBRE || this == ENERO || this == FEBRERO);
    }

    public boolean esDeOtono(boolean norte) {
        return norte ? (this == SEPTIEMBRE || this == OCTUBRE || this == NOVIEMBRE)
                     : (this == MARZO || this == ABRIL || this == MAYO);
    }
}
```


