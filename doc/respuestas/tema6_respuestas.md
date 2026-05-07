# TEMA 6. Genericidad

## 1. Empleando `void*` en C o `Object` en Java...

En C, se puede emplear `void*` para crear estructuras de datos capaces de almacenar cualquier tipo de dato, ya que este tipo de puntero es genérico y puede apuntar a cualquier tipo. Sin embargo, es necesario realizar conversiones explícitas (casting) al recuperar el dato, lo que introduce riesgos si se realiza incorrectamente.

Ejemplo en C:

```c
#include <stdio.h>

void imprimir(void* dato, char tipo) {
    if (tipo == 'i') {
        printf("%d\n", *(int*)dato);
    } else if (tipo == 'f') {
        printf("%f\n", *(float*)dato);
    }
}

int main() {
    int a = 10;
    float b = 3.14;

    imprimir(&a, 'i');
    imprimir(&b, 'f');
}
````

En Java, se consigue algo similar utilizando `Object`, ya que todas las clases heredan de él. Esto permite almacenar distintos tipos en una misma estructura, como un array de `Object`.

```java
Object[] datos = new Object[3];
datos[0] = "Hola";
datos[1] = 10;
datos[2] = 3.14;

for (Object o : datos) {
    System.out.println(o);
}
```

En ambos casos, se logra flexibilidad, pero se pierde seguridad de tipos, lo que obliga a tener precaución al recuperar los datos almacenados.

***

## 2. ¿Qué significa la programación genérica?

La programación genérica consiste en escribir código que puede trabajar con distintos tipos de datos sin necesidad de duplicar la lógica para cada tipo concreto. Se basa en parametrizar tipos, de manera que una misma estructura o algoritmo pueda adaptarse a distintos tipos en tiempo de compilación.

Este enfoque mejora la reutilización del código y reduce errores, ya que permite aplicar comprobaciones de tipos en tiempo de compilación. En lugar de depender de tipos generales como `Object`, se definen estructuras que operan con tipos concretos especificados por el usuario.

El ejemplo anterior con `void*` o `Object` puede considerarse una forma muy básica de genericidad, pero incompleta. Aunque permite almacenar cualquier tipo, no ofrece seguridad de tipos ni comprobación en compilación.

Por tanto, la programación genérica moderna va más allá, proporcionando mecanismos que combinan flexibilidad con seguridad, evitando errores en tiempo de ejecución.

***

## 3. Problemas con `void*` y `Object`

El principal problema al usar `void*` o `Object` es la pérdida del chequeo de tipos en tiempo de compilación. Esto implica que el compilador no puede verificar que las conversiones realizadas sean válidas, lo que aumenta el riesgo de errores.

En C, el uso de `void*` obliga a realizar casting manual, y un error en este proceso puede provocar comportamientos indefinidos, como accesos incorrectos a memoria. Esto convierte el código en menos seguro y más difícil de mantener.

En Java, aunque es más seguro que C, el uso de `Object` requiere downcasting al recuperar los valores. Si el tipo no coincide, se produce una excepción (`ClassCastException`) en tiempo de ejecución.

En conjunto, estos problemas motivaron la introducción de mecanismos de programación genérica más seguros, como los generics en Java o templates en C++.

***

## 4. ¿Qué son los parámetros de tipo?

Los parámetros de tipo son una forma de introducir tipos como parámetros en clases, interfaces o métodos. Permiten definir estructuras que no están ligadas a un tipo concreto, sino que reciben ese tipo como argumento al ser utilizadas.

Por ejemplo, en Java se puede definir una clase `Caja<T>` donde `T` representa el tipo del dato almacenado. Este tipo se especifica cuando se instancia la clase, permitiendo reutilizar la misma clase para distintos tipos.

Esto mejora directamente la seguridad de tipos, ya que el compilador puede verificar que se utilizan los tipos correctos sin necesidad de casting explícito. Además, el código resulta más claro y expresivo.

En definitiva, los parámetros de tipo son la base de la programación genérica moderna y permiten combinar flexibilidad con seguridad.

***

## 5. Ejemplo en Java y C++

En Java:

```java
import java.util.ArrayList;
import java.util.List;

List<String> lista = new ArrayList<>();
lista.add("Hola");
lista.add("Mundo");

for (String s : lista) {
    System.out.println(s.toUpperCase());
}
```

Aquí se observa que no es necesario hacer casting, ya que el compilador garantiza que todos los elementos son `String`.

En C++:

```cpp
#include <iostream>
#include <vector>
#include <string>
using namespace std;

int main() {
    vector<string> v;
    v.push_back("Hola");
    v.push_back("Mundo");

    for (auto s : v) {
        cout << s << endl;
    }
}
```

En ambos casos, se logra que la estructura de datos solo acepte `String`, y que al recorrerla se tenga la seguridad del tipo. Esto elimina errores y simplifica el código respecto a soluciones sin genericidad.

***

## 6. Funcionamiento interno

Cuando se usan parámetros de tipo, el comportamiento varía según el lenguaje. En Java, el compilador aplica un proceso llamado *type erasure*, en el cual elimina la información de tipos genéricos en tiempo de compilación, sustituyéndolos por su tipo base (generalmente `Object`).

Esto implica que, en tiempo de ejecución, no existe información sobre el tipo genérico, aunque el compilador sí haya realizado comprobaciones previas. Esto permite mantener compatibilidad con versiones anteriores del lenguaje.

En C++, se utiliza un enfoque distinto llamado instanciación de plantillas. El compilador genera una versión concreta del código para cada tipo utilizado, lo que permite mantener información de tipo en tiempo de ejecución.

La principal diferencia es que Java prioriza compatibilidad y simplicidad, mientras que C++ genera código especializado para cada tipo, lo que puede mejorar el rendimiento pero aumentar el tamaño del binario.

***

## 7. Clase `Par`

Se puede definir una clase genérica con dos tipos:

```java
class Par<T, U> {
    private T primero;
    private U segundo;

    public Par(T primero, U segundo) {
        this.primero = primero;
        this.segundo = segundo;
    }

    public T getPrimero() { return primero; }
    public U getSegundo() { return segundo; }
}
```

Ejemplo de uso:

```java
public static Par<Double, Double> calcular(double[] datos) {
    double suma = 0;
    for (double d : datos) suma += d;
    double media = suma / datos.length;

    double var = 0;
    for (double d : datos) var += Math.pow(d - media, 2);
    double desviacion = Math.sqrt(var / datos.length);

    return new Par<>(media, desviacion);
}
```

Esto permite devolver dos valores con tipos bien definidos sin necesidad de crear clases específicas para cada caso.

***

## 8. Método genérico

Ejemplo con `Object`:

```java
public static Object seleccionaUno(Object a, Object b) {
    return Math.random() < 0.5 ? a : b;
}
```

Este enfoque obliga a hacer casting al usar el resultado y no garantiza que ambos parámetros sean del mismo tipo.

Versión genérica:

```java
public static <T> T seleccionaUno(T a, T b) {
    return Math.random() < 0.5 ? a : b;
}
```

Aquí se evita el casting y se garantiza que ambos parámetros son del mismo tipo, ya que el compilador lo verifica. Esto mejora la seguridad y claridad del código.

***

## 9. Restricciones de tipo

En Java se pueden restringir los parámetros de tipo usando `extends`. Por ejemplo:

```java
class Punto<T extends Number> {
    private T x, y;

    public T getX() { return x; }
    public T getY() { return y; }
}
```

Sin generics:

```java
class Punto {
    private Number x, y;
}
```

La versión genérica permite saber el tipo concreto (por ejemplo `Integer`, `Double`). Tras compilación, debido a *type erasure*, el tipo real es `Number`.

Esto implica que, aunque en tiempo de compilación se tenga información precisa, en ejecución se pierde esa especificidad.

***

## 10. Comparación de soluciones

La solución sin generics permite mezclar tipos distintos en las coordenadas, por ejemplo un `Integer` y un `Double`. Esto puede generar incoherencias en el tratamiento de los datos.

En cambio, la solución con generics obliga a que ambas coordenadas sean del mismo tipo (`T`), reforzando la consistencia del objeto.

Además, en la versión sin generics, los métodos como `getX()` devuelven `Number`, lo que obliga a casting para operaciones específicas. En la versión con generics, devuelven `T`, proporcionando mayor precisión.

Por tanto, los generics mejoran tanto la seguridad como la expresividad del código.

***

## 11. Ejemplo avanzado sin `instanceof`

Se puede usar generics para forzar que ambos puntos sean del mismo tipo:

```java
public interface Punto<T extends Punto<T>> {
    double distanciaA(T p);
}
```

Implementación:

```java
public class Punto2D implements Punto<Punto2D> {
    private final double x, y;

    public double distanciaA(Punto2D p) {
        return Math.sqrt(Math.pow(x - p.x, 2) + Math.pow(y - p.y, 2));
    }
}
```

Se elimina la necesidad de `instanceof` y casting, ya que el compilador garantiza que el tipo es correcto. Esto mejora claridad y seguridad.

***

## 12. Covarianza y arrays

`List<String>` no es subtipo de `List<Object>` porque los genéricos en Java son invariantes. Esto evita errores en tiempo de ejecución al insertar elementos de tipo incorrecto.

Sin embargo, los arrays sí son covariantes: `String[]` es subtipo de `Object[]`. Esto puede provocar errores en ejecución, como intentar insertar un `Integer` en un array de `String`.

Un tipo es covariante si permite sustituir subtipos, contravariante si permite supertipos, e invariante si no permite ninguna de estas sustituciones.

***

## 13. Wildcards

Un wildcard (`?`) representa un tipo desconocido. Permite escribir código más flexible al trabajar con jerarquías de tipos genéricos.

`List<? extends T>` se usa cuando se quiere leer datos de una lista (covarianza). `List<? super T>` se usa cuando se quiere escribir datos (contravarianza).

Ejemplos:

```java
public static double suma(List<? extends Number> lista) {
    double s = 0;
    for (Number n : lista) s += n.doubleValue();
    return s;
}
```

```java
public static void añadir(List<? super Integer> lista) {
    lista.add(1);
    lista.add(2);
}
```

Esto permite trabajar con genéricos de forma segura y flexible, respetando restricciones de tipos.

```
```
