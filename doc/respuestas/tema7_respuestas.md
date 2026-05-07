# TEMA 7. Aspectos funcionales

## 1. ¿Qué es un puntero a una función?

Un puntero a una función en C es una variable que almacena la dirección de memoria de una función. Esto permite tratar funciones como datos, pasarlas como parámetros o almacenarlas en estructuras. Es un mecanismo muy potente que introduce cierto grado de abstracción, aunque sin llegar al nivel de los lenguajes modernos.

Se utiliza especialmente cuando se desea parametrizar el comportamiento de un algoritmo, por ejemplo, indicar qué función debe aplicarse en cada caso. A diferencia de otros lenguajes, en C esta funcionalidad requiere una sintaxis más compleja y menos intuitiva.

```c
#include <stdio.h>
#include <ctype.h>

void mayusculas(char* str) {
    for (int i = 0; str[i]; i++) {
        str[i] = toupper(str[i]);
    }
}

int main() {
    void (*aMayusculas)(char*) = mayusculas;

    char texto[] = "hola";
    aMayusculas(texto);

    printf("%s\n", texto);
}
````

En este ejemplo, `aMayusculas` es un puntero que permite invocar la función indirectamente.

***

## 2. ¿Qué es una función lambda?

Una función lambda es una función anónima, es decir, una función que no tiene nombre y que puede definirse directamente en el lugar donde se necesita. Permite escribir código más conciso y flexible, especialmente cuando se trata de funciones pequeñas.

En JavaScript:

```javascript
let aMayusculas = (str) => str.toUpperCase();
console.log(aMayusculas("hola"));
```

En Java:

```java
import java.util.function.Function;

Function<String, String> aMayusculas = s -> s.toUpperCase();
System.out.println(aMayusculas.apply("hola"));
```

En ambos casos, se puede observar que la función se asigna directamente a una variable. Esto simplifica el código respecto a la definición tradicional de funciones.

***

## 3. Paradigma funcional

El paradigma funcional es un enfoque de programación que se basa en el uso de funciones como elemento principal. En lugar de modificar estados, se centra en aplicar funciones a datos, favoreciendo la inmutabilidad y la composición.

Java es considerado multi-paradigma porque, aunque es principalmente orientado a objetos, incorpora características funcionales desde Java 8, como lambdas, streams y funciones de orden superior.

Que las funciones sean ciudadanos de primera clase implica que pueden asignarse a variables, pasarse como parámetros y devolverse como resultado, igual que cualquier otro valor.

***

## 4. Sintaxis de lambda en Java

La sintaxis general es:

```java
(parámetros) -> { cuerpo }
```

Si solo hay un parámetro, se pueden omitir los paréntesis. Si el cuerpo tiene una única expresión, se pueden omitir las llaves y el `return`.

Esto permite escribir funciones de forma muy compacta. Por ejemplo:

```java
s -> s.toUpperCase()
```

En este caso, se define una función que recibe un `String` y devuelve otro.

***

## 5. Funciones como parámetros

En Java:

```java
import java.util.function.Function;

public static String transformar(String s, Function<String, String> f) {
    return f.apply(s);
}
```

En JavaScript:

```javascript
function transformar(s, f) {
    return f(s);
}
```

Este diseño permite desacoplar el comportamiento del método, haciendo que sea más reutilizable.

***

## 6. Lambda inline

En Java:

```java
transformar("hola", s -> new StringBuilder(s).reverse().toString());
```

En JavaScript:

```javascript
transformar("hola", s => s.split("").reverse().join(""));
```

Se observa que la función se define directamente en la llamada, lo que mejora la expresividad del código.

***

## 7. Closure

Un closure es una función que captura variables del contexto donde fue creada. Estas variables permanecen accesibles incluso cuando la función se ejecuta fuera de ese contexto.

Ejemplo:

```java
String sufijo = "!!!";

Function<String, String> f = s -> s + sufijo;
System.out.println(f.apply("hola"));
```

Aquí la lambda accede a `sufijo`, que está fuera de ella. Esto demuestra la capacidad de capturar el entorno.

***

## 8. Diferencias con punteros a función

Las lambdas son más potentes que los punteros a función, ya que pueden capturar variables del entorno (closures), cosa que los punteros en C no hacen directamente.

Además, las lambdas están integradas en el sistema de tipos, lo que permite mayor seguridad. En cambio, los punteros en C son más básicos y requieren más control manual.

Por tanto, las lambdas representan una evolución más segura y expresiva.

***

## 9. Devolver funciones

```java
import java.util.function.Function;

public static Function<Double, Double> crearDescuento(double porcentaje) {
    return precio -> precio * (1 - porcentaje);
}
```

Uso:

```java
Function<Double, Double> d10 = crearDescuento(0.10);
System.out.println(d10.apply(100.0));
```

Aquí la lambda captura `porcentaje`, formando un closure que mantiene ese valor.

***

## 10. Interfaz funcional

Una interfaz funcional es una interfaz que tiene un único método abstracto. Es el tipo que permite representar funciones en Java.

Puede tener otros métodos por defecto o estáticos, pero solo uno abstracto. Esto permite que sea compatible con expresiones lambda.

Ejemplo: `Function<T, R>` es una interfaz funcional.

***

## 11. Crear interfaz funcional

```java
@FunctionalInterface
interface Transformador {
    String transformar(String s);
}
```

Esta interfaz define una función que transforma una cadena en otra.

Permite usar lambdas compatibles con esa firma.

***

## 12. Versión genérica

```java
@FunctionalInterface
interface Transformador<T, R> {
    R transformar(T valor);
}
```

Ejemplo:

```java
Transformador<Double, Integer> redondear = d -> (int) Math.round(d);
```

Esto permite reutilizar la interfaz con distintos tipos.

***

## 13. Interfaces funcionales predefinidas

Java incluye varias interfaces funcionales:

*   `Function<T, R>`
*   `Consumer<T>`
*   `Supplier<T>`
*   `Predicate<T>`
*   `BiFunction<T, U, R>`

Estas permiten representar funciones comunes sin necesidad de definir nuevas.

***

## 14. forEach

```java
import java.util.*;

List<Integer> lista = Arrays.asList(1, -2, 3);

lista.forEach(n -> {
    if (n > 0) {
        System.out.println("Positivo: " + n);
    }
});
```

Se usa un estilo funcional que sustituye al bucle tradicional.

***

## 15. PECS

PECS significa "Producer Extends, Consumer Super". Indica cuándo usar `extends` y `super`.

`Consumer<? super T>` permite aceptar tipos más generales que `T`, lo que da flexibilidad al consumir datos.

Aplicado a `transformar`, permite aceptar funciones más generales sin perder seguridad.

***

## 16. Referencias a métodos

JavaScript:

```javascript
class Persona {
    constructor(nombre) { this.nombre = nombre; }
    saludar() { console.log("Hola " + this.nombre); }
}

let p = new Persona("Ana");
let f = p.saludar.bind(p);
f();
```

Java:

```java
class Persona {
    String nombre;
    Persona(String n) { nombre = n; }
    void saludar() { System.out.println("Hola " + nombre); }
}

Persona p = new Persona("Ana");
Runnable r = p::saludar;
r.run();
```

***

## 17. Tipos de referencias

```java
// Estático
Integer::parseInt

// Constructor
ArrayList::new

// Instancia concreta
p::saludar

// Instancias arbitrarias
String::toUpperCase
```

Cada tipo permite referenciar distintos tipos de métodos.

***

## 18. Ordenar lista

Manual:

```java
Collections.sort(lista, (a, b) -> {
    if (a.edad != b.edad) return a.edad - b.edad;
    return a.nombre.compareTo(b.nombre);
});
```

Con Comparator:

```java
Collections.sort(lista,
    Comparator.comparingInt((Persona p) -> p.edad)
              .thenComparing(p -> p.nombre));
```

La segunda versión es más clara y reutilizable, aprovechando utilidades estándar.

```
```
