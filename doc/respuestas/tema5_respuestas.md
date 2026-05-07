# Tema 5. Polimorfismo

## 1. Brevemente, ¿qué es el **"polimorfismo"** y para qué sirve en programación orientada a objetos? ¿qué es la **"sobreescritura"** de métodos?

El polimorfismo es un concepto fundamental de la programación orientada a objetos que permite tratar objetos de distintas clases relacionadas como si fueran del mismo tipo base. Se basa en la idea de que una misma operación (por ejemplo, un método) puede tener comportamientos diferentes dependiendo del objeto concreto sobre el que se invoca. Esto facilita escribir código más general y flexible, donde se trabaja con abstracciones en lugar de implementaciones concretas.

Se utiliza principalmente para permitir que diferentes clases derivadas respondan de forma distinta a una misma llamada de método. Gracias a ello se puede reducir el acoplamiento entre componentes y hacer el código más extensible, ya que se pueden añadir nuevas clases sin modificar el código existente que usa el tipo base.

La sobreescritura (overriding) de métodos consiste en redefinir en una subclase un método que ya existe en la clase base, proporcionando una implementación distinta. Para que exista sobreescritura, el método debe tener la misma firma (nombre y parámetros) que en la clase original.

Este mecanismo es el que hace posible el polimorfismo en tiempo de ejecución, ya que cuando se invoca un método sobre una referencia de una clase base, el comportamiento dependerá realmente del tipo del objeto al que hace referencia.

---

## 2. ¿En qué consiste la **"ligadura dinámica"** o **"enlace tardío"**? ¿qué relación tiene con el polimorfismo? ¿hay que indicarlos explícitamente al programar o depende esto del lenguaje? Compara C++ y Java. Indicalo después también para Python.

La ligadura dinámica (o enlace tardío) es el mecanismo mediante el cual la decisión de qué implementación de un método se ejecuta se toma en tiempo de ejecución, en lugar de en tiempo de compilación. Esto significa que el programa no determina de antemano qué versión de un método se utilizará, sino que lo hace en función del tipo real del objeto.

Este concepto está directamente relacionado con el polimorfismo, ya que permite que una misma llamada a método tenga distintos comportamientos dependiendo del objeto concreto. Sin ligadura dinámica, no sería posible implementar el polimorfismo en su forma más flexible.

En C++, la ligadura dinámica no es automática: es necesario indicar explícitamente qué métodos son polimórficos usando la palabra clave `virtual`. Si no se hace, se utiliza enlace estático. En cambio, en Java todos los métodos (excepto los `static`, `final` y `private`) utilizan ligadura dinámica por defecto, por lo que el polimorfismo es más natural.

En Python, el enlace es dinámico por naturaleza, ya que es un lenguaje dinámico. No es necesario declarar nada especial: las llamadas a métodos siempre se resuelven en tiempo de ejecución según el tipo del objeto, lo que facilita el uso del polimorfismo de forma implícita.

---

## 3. Pon un ejemplo sencillo en Java...

A continuación se muestra un ejemplo simple donde se define una clase base `Soldado` y dos subclases que modifican su comportamiento:

```java
class Soldado {
    public void saludar() {
        System.out.println("Soldado saludando");
    }
}

class Zapador extends Soldado {
    @Override
    public void saludar() {
        System.out.println("Zapador saludando");
    }
}

class Artillero extends Soldado {
    // usa implementación base
}
````

El polimorfismo se observa al trabajar con referencias del tipo base:

```java
public class Main {
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

Aunque el array es de tipo `Soldado`, cada objeto responde de forma distinta a `saludar()`. Esto demuestra cómo el comportamiento se decide en tiempo de ejecución según el tipo real del objeto.

***

## 4. ¿puedo invocar el método base...?

Sí, es posible invocar el método de la clase base desde la subclase al sobreescribir un método. Esto resulta útil cuando se desea reutilizar parte del comportamiento original y añadir funcionalidad adicional sin reemplazar completamente la implementación.

En Java, se utiliza la palabra clave `super` para acceder a los métodos de la clase padre. Esto permite llamar explícitamente a la versión original del método antes o después de añadir comportamiento extra.

Ejemplo:

```java
class Zapador extends Soldado {
    @Override
    public void saludar() {
        super.saludar();
        System.out.println("ZAPADOR A SUS ORDENES");
    }
}
```

En este caso, primero se ejecuta el saludo estándar del `Soldado` y luego se añade un comportamiento específico del `Zapador`. La palabra clave utilizada para acceder al método base es `super`.

***

## 5. Restricciones en overriding y diferencia con overloading

Al sobreescribir un método en Java, deben respetarse ciertas reglas: la firma del método debe ser la misma (nombre y parámetros), y el tipo de retorno debe ser compatible (igual o covariante). Además, no se pueden reducir los modificadores de acceso (por ejemplo, no se puede pasar de `public` a `protected`).

La diferencia entre sobreescritura (overriding) y sobrecarga (overloading) es importante. La sobreescritura ocurre entre una clase base y una subclase y redefine un método existente, mientras que la sobrecarga consiste en tener varios métodos con el mismo nombre pero diferentes parámetros dentro de la misma clase.

La anotación `@Override` se utiliza para indicar explícitamente que se está sobreescribiendo un método. Aunque no es obligatoria, es altamente recomendable porque permite al compilador detectar errores, como firmas incorrectas o métodos que en realidad no coinciden con los de la clase base.

***

## 6. Uso temprano del polimorfismo

El polimorfismo se emplea desde fases muy tempranas en Java, incluso sin ser plenamente consciente de ello. Cada vez que se sobreescribe un método de una clase base, se está aplicando polimorfismo en tiempo de ejecución.

Ejemplos típicos son la redefinición de métodos como `toString()` o `equals()`, que pertenecen a la clase `Object`. Al redefinir estos métodos, se está adaptando su comportamiento para una clase específica, lo cual es una forma directa de polimorfismo.

Por tanto, sí, en el momento en que se sobreescribe cualquiera de estos métodos, ya se está utilizando polimorfismo, aunque sea en ejemplos básicos. Esto demuestra que el concepto está presente desde el inicio del aprendizaje de Java.

***

## 7. Clases y métodos abstractos

Una clase abstracta es una clase que no puede ser instanciada directamente y que sirve como base para otras clases. Puede contener tanto métodos implementados como métodos abstractos, que no tienen implementación y deben ser definidos por las subclases.

Un método abstracto es aquel que se declara sin cuerpo y obliga a las clases derivadas a proporcionar su propia implementación. Este enfoque permite definir comportamientos generales sin especificar los detalles concretos.

Ejemplo:

```java
abstract class Soldado {
    public void saludar() {
        System.out.println("Saludo general");
    }

    public abstract void atacar();
}

class Zapador extends Soldado {
    @Override
    public void atacar() {
        System.out.println("Zapador colocando explosivos");
    }
}
```

La palabra clave `abstract` debe colocarse tanto en la clase (si contiene métodos abstractos) como en los métodos que no tienen implementación.

***

## 8. Palabra clave `final`

En Java, la palabra clave `final` tiene distintos usos dependiendo del contexto. En el caso de clases, indica que no pueden ser heredadas. En el caso de métodos, indica que no pueden ser sobreescritos por subclases.

Esto tiene una relación directa con el polimorfismo, ya que limita su uso. Si un método es `final`, no puede participar en polimorfismo basado en sobreescritura, ya que no se puede redefinir su comportamiento en una subclase.

Un ejemplo claro en la API estándar de Java es la clase `String`, que es `final`. Esto impide que se herede de ella, lo cual tiene implicaciones importantes para la seguridad y consistencia del lenguaje.

***

## 9. Interfaces

Las interfaces en Java son un tipo especial que define un contrato de métodos que una clase debe implementar. No contienen estado (salvo constantes) y tradicionalmente sus métodos eran abstractos (aunque desde Java 8 pueden tener implementaciones por defecto).

Son similares a clases abstractas, pero más restrictivas. La principal diferencia es que una clase puede implementar múltiples interfaces, pero solo puede heredar de una clase.

Esto permite simular una forma de herencia múltiple, facilitando la combinación de comportamientos sin los problemas asociados a la herencia múltiple de clases.

***

## 10. Ejemplo con Punto y Línea

Se puede diseñar una jerarquía donde `Punto` sea abstracto:

```java
abstract class Punto {
    public abstract double calcularDistanciaA(Punto otro);
}
```

Implementaciones:

```java
class Punto2D extends Punto {
    double x, y;

    public double calcularDistanciaA(Punto otro) {
        if (otro instanceof Punto2D) {
            Punto2D p = (Punto2D) otro;
            return Math.sqrt(Math.pow(x - p.x, 2) + Math.pow(y - p.y, 2));
        }
        throw new IllegalArgumentException("Tipo incompatible");
    }
}

class Punto3D extends Punto {
    double x, y, z;

    public double calcularDistanciaA(Punto otro) {
        if (otro instanceof Punto3D) {
            Punto3D p = (Punto3D) otro;
            return Math.sqrt(Math.pow(x - p.x, 2)
                + Math.pow(y - p.y, 2)
                + Math.pow(z - p.z, 2));
        }
        throw new IllegalArgumentException("Tipo incompatible");
    }
}
```

Clase `Linea`:

```java
class Linea {
    Punto a, b;

    public Linea(Punto a, Punto b) {
        this.a = a;
        this.b = b;
    }

    public double longitud() {
        return a.calcularDistanciaA(b);
    }
}
```

Se demuestra cómo `Linea` no necesita conocer si los puntos son 2D o 3D, gracias al polimorfismo.

***

## 11. Herencia de interfaces

La herencia de interfaces en Java permite que una interfaz extienda otra, heredando sus métodos. Esto facilita la construcción de contratos más complejos a partir de otros más simples.

Java permite la herencia múltiple de interfaces, es decir, una interfaz puede extender varias interfaces simultáneamente. Esto da flexibilidad en el diseño sin los inconvenientes de la herencia múltiple de clases.

Ejemplo:

```java
interface Fichero {
    String leer();
}

interface FicheroEscribible extends Fichero {
    void escribir(String contenido);
    void borrar();
}
```

En este caso, `FicheroEscribible` hereda el método `leer()` y añade nuevas funcionalidades, ampliando el contrato original.

```
```
