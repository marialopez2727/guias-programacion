# TEMA 1. Clases y objetos

## 1. ¿Cuáles son las cuatro características básicas de la programación orientada a objetos? Describe brevemente cada una

La programación orientada a objetos (POO) se apoya en cuatro pilares fundamentales: **encapsulamiento, abstracción, herencia y polimorfismo**. El encapsulamiento consiste en proteger los datos internos de un objeto, de modo que solo puedan modificarse a través de métodos controlados. Este mecanismo ayuda a prevenir errores y a mantener la integridad del estado de los objetos.

La abstracción permite centrarse en los aspectos esenciales de una entidad, ocultando detalles que no son relevantes para su uso. Esto simplifica el manejo de estructuras complejas. Por su parte, la herencia introduce la posibilidad de crear nuevas clases basadas en otras existentes, lo que facilita la reutilización de código y la extensión de funcionalidades.

Finalmente, el polimorfismo habilita que distintos objetos puedan responder de forma diferente a un mismo mensaje o llamada de método. Este mecanismo posibilita escribir código más flexible y genérico, especialmente útil cuando se trabajan con jerarquías de clases y comportamientos especializados.

---

## 2. Cita cuatro lenguajes populares que permitan la programación orientada a objetos

Algunos lenguajes ampliamente usados que soportan programación orientada a objetos son **Java, C++, Python y C#**. Cada uno adopta el paradigma con matices distintos, pero permiten la definición de clases, objetos y métodos de forma estructurada.

Java y C# tienden a ser más estrictos con el modelo de objetos y la gestión de memoria automática, mientras que C++ combina POO con capacidades de bajo nivel similares al C tradicional. Python adopta una POO más flexible y dinámica, permitiendo modificar clases y objetos en tiempo de ejecución. Estos lenguajes son referencia habitual para aprender y aplicar los fundamentos de este paradigma.

---

## 3. Los paradigmas anteriores a la POO, ¿Qué es la programación estructurada? y la programación modular?

La programación estructurada organiza el código mediante estructuras de control como secuencia, selección e iteración. Su objetivo es reducir el uso de saltos incontrolados como `goto`, dando lugar a programas más legibles y mantenibles.

La programación modular amplía este concepto dividiendo el programa en módulos independientes, cada uno de ellos encargado de una funcionalidad concreta. Esta separación favorece la reutilización y comprensión del código, pues cada módulo actúa como una unidad bien delimitada. Aunque la modularidad no implica necesariamente objetos, sentó las bases para organizar el código en torno a clases.

---

## 4. ¿Qué tres elementos definen a un objeto en programación orientada a objetos?

Un objeto se caracteriza por **estado, comportamiento e identidad**. El estado está formado por los datos o atributos que describen las características del objeto. El comportamiento se define mediante métodos que representan las operaciones que el objeto puede realizar. La identidad distingue a un objeto de otros, incluso si tienen el mismo estado.

---

## 5. ¿Qué es una clase? ¿Es lo mismo que un objeto? ¿Qué es una instancia? ¿Todos los lenguajes orientados a objetos manejan el concepto de clase?

Una clase es una plantilla que describe los atributos y métodos que tendrán los objetos creados a partir de ella. No es un objeto en sí, sino un modelo que define su forma y comportamiento. Los objetos son entidades concretas que existen en la memoria durante la ejecución del programa.

Una instancia es un objeto creado a partir de una clase. Aunque la mayoría de los lenguajes se basan en clases, algunos como JavaScript permiten crear objetos sin necesidad de una clase explícita.

---

## 6. ¿Dónde se almacenan en memoria los objetos? ¿Es igual en todos los lenguajes? ¿Qué es la recolección de basura?

En Java, los objetos se almacenan en el **heap**, gestionado automáticamente por la JVM, mientras que las referencias se almacenan en la pila. C++ permite crear objetos en pila o heap de forma manual.

La **recolección de basura** es un proceso automático que libera memoria de objetos que ya no se usan, evitando fugas de memoria.

---

## 7. ¿Qué es un método? ¿Qué es la sobrecarga de métodos?

Un método es una función asociada a una clase que define un comportamiento específico de sus objetos. La sobrecarga permite definir varios métodos con el mismo nombre pero diferentes parámetros, facilitando la legibilidad y reutilización de nombres coherentes.

---

## 8. Ejemplo mínimo de clase en Java: Punto

```java
public class Punto {
    double x;
    double y;

    double calculaDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }

    public static void main(String[] args) {
        Punto p = new Punto();
        p.x = 3;
        p.y = 4;
        System.out.println(p.calculaDistanciaAOrigen());
    }
}
```

---

## 9. Punto de entrada en Java, palabra static y combinación con final

El método `main` es el punto de entrada: `public static void main(String[] args)`. `static` permite ejecutar métodos sin instanciar la clase. `final` junto con `static` define constantes inmutables asociadas a la clase.

---

## 10. Compilación con javac, ejecución con java, bytecode y máquina virtual

`javac` compila a `.class` (bytecode), que se ejecuta con `java` sobre la JVM. Esto permite portabilidad y separación de compilación y ejecución.

---

## 11. ¿Qué es new? ¿Qué es un constructor? Ejemplo en Empleado

`new` crea objetos y reserva memoria en el heap. Un constructor inicializa atributos del objeto.

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

## 12. ¿Qué es la referencia this? Ejemplo en Punto

`this` accede al objeto actual y distingue atributos de parámetros con nombres iguales.

```java
public double distanciaA(Punto otro) {
    double dx = this.x - otro.x;
    double dy = this.y - otro.y;
    return Math.sqrt(dx * dx + dy * dy);
}
```

---

## 13. Añadir método distanciaA a Punto

```java
double distanciaA(Punto p) {
    double dx = this.x - p.x;
    double dy = this.y - p.y;
    return Math.sqrt(dx * dx + dy * dy);
}
```

---

## 14. ¿El paso del objeto es por copia o referencia? ¿Qué ocurre con un int?

Objetos se pasan por **referencia**, tipos primitivos como `int` se pasan por **valor**.

---

## 15. ¿Qué es el método toString()? Ejemplo en Punto

`toString()` devuelve una representación textual de un objeto.

```java
@Override
public String toString() {
    return "Punto(" + x + ", " + y + ")";
}
```

---

## 16. ¿Una clase es como un struct en C?

Un `struct` agrupa variables pero carece de métodos, encapsulamiento, herencia y polimorfismo. Para simularlo, habría que asociarle funciones externas.

---

## 17. Emular Punto con struct en C

```c
#include <math.h>

typedef struct {
    double x;
    double y;
} Punto;

double distanciaAOrigen(Punto* p) {
    return sqrt(p->x * p->x + p->y * p->y);
}
```
