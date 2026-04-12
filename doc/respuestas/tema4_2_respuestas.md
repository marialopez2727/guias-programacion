## 1. En orientación a objetos, ¿qué es la **herencia** y su relación con "A es-un B"?. Explica las dos implicaciones principales: (1) **compatibilidad de tipos** y (2) **herencia de estado y comportamiento**. Pon un ejemplo en Java muy sencillo, donde un `Soldado` tiene un `nombre` (privado) y un método `saludar()` que muestra su nombre. Hay dos subtipos: un `Artillero`, que es capaz de disparar cohetes y un `Zapador` que pone minas, ambos heredan el atributo nombre y la capacidad de saludar. Además, y de forma específica, el artillero tiene un número de cohetes y el zapador un número de minas, accesibles mediante "getters" específicos. Respecto a la compatibilidad de tipos, aprovechémosla: crea un array de `Soldado`, mete varios de distinto tipo (son todos compatibles con `Soldado`). Recórrela y que todos te saluden.

### Respuesta

La herencia es un mecanismo de la orientación a objetos que permite definir una nueva clase a partir de otra ya existente. Se expresa conceptualmente como una relación “A es-un B”, donde la subclase (A) es una especialización de la superclase (B). Esto implica que todo objeto de la subclase puede tratarse como si fuera un objeto de la superclase.

La primera implicación importante es la compatibilidad de tipos. Un objeto de una subclase puede almacenarse en una referencia del tipo de la superclase, lo que permite trabajar de forma uniforme con objetos distintos que comparten una jerarquía común. Esta compatibilidad es la base del polimorfismo.

La segunda implicación es la herencia de estado y comportamiento. La subclase hereda los atributos y métodos accesibles de la superclase, reutilizando código ya existente y pudiendo añadir o especializar comportamientos propios.

```java
public class Soldado {
    private String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Hola, soy " + nombre);
    }
}

public class Artillero extends Soldado {
    private int cohetes;

    public Artillero(String nombre, int cohetes) {
        super(nombre);
        this.cohetes = cohetes;
    }

    public int getCohetes() {
        return cohetes;
    }
}

public class Zapador extends Soldado {
    private int minas;

    public Zapador(String nombre, int minas) {
        super(nombre);
        this.minas = minas;
    }

    public int getMinas() {
        return minas;
    }
}

public class Main {
    public static void main(String[] args) {
        Soldado[] soldados = new Soldado[] {
            new Artillero("Luis", 5),
            new Zapador("Ana", 3)
        };

        for (Soldado s : soldados) {
            s.saludar();
        }
    }
}
````

## 2. Al crear los soldados concretos, ¿cuántos constructores se ejecutan y en qué orden? ¿Qué significa `super` dentro de un constructor? Si la clase base no tiene visible el constructor sin parámetros, ¿debo llamar a `super` siempre?

### Respuesta

Al crear un objeto de una subclase, se ejecutan los constructores de toda la jerarquía de herencia. En primer lugar se ejecuta el constructor de la clase base y, a continuación, el de la subclase más concreta. Este orden garantiza que la parte “base” del objeto esté correctamente inicializada antes de añadir el estado específico.

La palabra clave `super` dentro de un constructor se utiliza para invocar explícitamente a un constructor de la superclase. Permite pasar parámetros y controlar qué constructor base se ejecuta, algo fundamental cuando la clase base necesita información para inicializarse correctamente.

Si la clase base no tiene un constructor sin parámetros accesible, es obligatorio llamar explícitamente a `super(...)` desde los constructores de la subclase. De lo contrario, el código no compilará, ya que Java intenta invocar automáticamente al constructor sin argumentos de la superclase y no lo encontrará.

## 3. Respecto a los objetos de subclases en memoria, los atributos privados de la superclase, ¿forman parte de una instancia de la subclase en memoria? En caso afirmativo ¿implica que se puedan usar desde el código de la subclase? Explícalo con el ejemplo de `Soldado` y alguna de sus subclases.

### Respuesta

Los atributos privados de la superclase forman parte físicamente de la instancia en memoria del objeto de la subclase. Un objeto `Artillero` contiene internamente todo el estado definido en `Soldado`, además de su propio estado específico.

Sin embargo, que los atributos existan en memoria no implica que sean accesibles desde el código de la subclase. El modificador `private` restringe el acceso únicamente al código definido dentro de la propia clase base.

En el ejemplo, el atributo `nombre` pertenece a cualquier `Artillero` o `Zapador`, pero no puede ser accedido directamente desde estas clases. El acceso debe realizarse a través de métodos públicos o protegidos definidos en `Soldado`, respetando así la encapsulación.

## 4. ¿Qué implica en términos de **extensibilidad** de código el hecho de que sean compatibles a nivel de tipos? Ilustra esto añadiendo un nuevo tipo de `Soldado` y demostrando que el código para pedir el saludo a todos los soldados no se modifica.

### Respuesta

La compatibilidad de tipos mejora notablemente la extensibilidad del código. Permite añadir nuevas subclases sin tener que modificar el código que trabaja con la superclase, siempre que ese código dependa únicamente del contrato común.

Esto facilita el cumplimiento del principio de abierto/cerrado: el sistema puede extenderse con nuevos comportamientos sin necesidad de cambiar el código existente, reduciendo el riesgo de introducir errores.

Al añadir un nuevo tipo de soldado, el recorrido del array y la llamada al método `saludar()` permanecen invariantes, ya que dicho método está definido en la superclase.

```java
public class Medico extends Soldado {
    public Medico(String nombre) {
        super(nombre);
    }
}
```

## 5. En Java, cuando trabajo con referencias y herencia. ¿Puedo tener una referencia del supertipo que apunte a objetos reales de un subtipo? ¿Puedo invocar con la referencia del supertipo a métodos públicos del subtipo? ¿En qué consiste el **"upcasting"** y el **"downcasting"**? ¿Qué es el `instanceof`? Pon un ejemplo de recorrido de un array de `Soldado`, comprobando que, si el objeto real es un `Artillero`, solicite el número de cohetes que tiene y los imprima.

### Respuesta

En Java es completamente válido que una referencia del supertipo apunte a un objeto real de un subtipo. Esta es precisamente la base del polimorfismo. Sin embargo, con esa referencia solo se pueden invocar métodos definidos en el tipo estático de la referencia.

El upcasting consiste en tratar un objeto de una subclase como si fuera de la superclase, y se realiza de forma implícita. El downcasting es la operación inversa y requiere una conversión explícita, ya que puede producir errores en tiempo de ejecución.

El operador `instanceof` permite comprobar el tipo real del objeto antes de hacer un downcasting seguro.

```java
for (Soldado s : soldados) {
    if (s instanceof Artillero) {
        Artillero a = (Artillero) s;
        System.out.println("Cohetes: " + a.getCohetes());
    }
}
```

## 6. Respecto a la ocultación de información y herencia, ¿qué significa acceso **"protegido"** de métodos y/o atributos? ¿Cómo se implementa en Java? Pon un ejemplo de uso de en la clase `Soldado` para que su nombre sea protegido y pueda usarse en el método de poner bombas del `Zapador`.

### Respuesta

El acceso protegido permite que los miembros de una clase sean accesibles desde las subclases, pero no desde código externo no relacionado. Es un punto intermedio entre `private` y `public`.

En Java se implementa mediante la palabra clave `protected`. Este modificador resulta útil cuando se desea que las subclases reutilicen o amplíen el comportamiento de la clase base sin exponer detalles internos al resto del sistema.

En el ejemplo, el nombre del soldado puede declararse como protegido para que el `Zapador` lo use internamente sin necesidad de romper la encapsulación.

```java
public class Soldado {
    protected String nombre;
}
```

## 7. En los lenguajes orientados a objetos ¿hay una **clase base** para todos los objetos? ¿Ocurre en todos los lenguajes? ¿Qué ocurre en Java?

### Respuesta

Algunos lenguajes orientados a objetos definen una clase base común para todos los objetos, mientras que otros no lo hacen o lo hacen de manera implícita. Esto depende del diseño del lenguaje.

En Java, todas las clases heredan directa o indirectamente de `Object`. Esto garantiza que todos los objetos compartan un conjunto mínimo de métodos comunes, como `toString`, `equals` o `hashCode`.

Este diseño simplifica la gestión genérica de objetos y permite tratar cualquier instancia de forma uniforme en muchas situaciones básicas del lenguaje.

## 8. ¿Qué es la **"herencia múltiple"**? ¿Existe en Java herencia múltiple?

### Respuesta

La herencia múltiple es un mecanismo que permite que una clase herede directamente de más de una clase base. Esto puede aumentar la reutilización, pero también introduce problemas de ambigüedad y complejidad.

Java no permite herencia múltiple de clases para evitar estos problemas. Sin embargo, sí permite implementar múltiples interfaces, lo que proporciona una forma controlada de heredar comportamientos abstractos.

De este modo, Java equilibra flexibilidad y simplicidad, evitando conflictos como el conocido “problema del diamante”.

## 9. Las excepciones en los lenguajes orientados a objetos son objetos. Por tanto, se pueden crear excepciones personalizadas. Pon un ejemplo en Java de una excepción personalizada (`UsuarioNoEncontradoException`), que sea *no controlada* y que además este compuesto con un `Usuario`, para saber qué `Usuario` dio el problema. Permite además que se pueda incluir la causa, es decir, sobrecarga el constructor para tener una versión que permita añadir la causa subyacente.

### Respuesta

Las excepciones personalizadas permiten modelar errores específicos del dominio de la aplicación. Al ser objetos, pueden componerse con otros objetos para aportar información adicional sobre la situación de error.

Una excepción no controlada se consigue heredando de `RuntimeException`. Esto hace que no sea obligatorio declararla ni capturarla, dejando la responsabilidad al diseño del sistema.

La inclusión de la causa permite encadenar excepciones, conservando información de bajo nivel junto al error de alto nivel.

```java
public class UsuarioNoEncontradoException extends RuntimeException {
    private final Usuario usuario;

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

## 10. Herencia vs. Composición. Se dice que no se debe emplear herencia simplemente por reutilizar código, es decir, que si quiero reutilizar código simplemente, no debo pensar en herencia como primera opción ¿por qué?

### Respuesta

La herencia establece una relación fuerte de dependencia semántica: un subtipo debe cumplir realmente la relación “es-un” respecto a su supertipo. Usarla solo para reutilizar código puede llevar a jerarquías artificiales y mal diseñadas.

Además, la herencia acopla fuertemente las clases, de modo que cambios en la superclase pueden afectar inesperadamente a todas las subclases. Esto dificulta el mantenimiento y la evolución del código.

La composición, en cambio, permite reutilizar código sin imponer relaciones rígidas ni comprometer el diseño conceptual.

## 11. Herencia vs. Composición. Se dice que se debe *"favorecer la composición frente a la herencia"*, ¿por qué?

### Respuesta

Favorecer la composición implica construir clases a partir de otras mediante referencias, en lugar de heredar comportamiento. Esto proporciona mayor flexibilidad y menor acoplamiento entre las partes del sistema.

La composición permite cambiar dinámicamente los componentes utilizados, mientras que la herencia fija la estructura en tiempo de compilación. Esto mejora la adaptabilidad del diseño ante nuevos requisitos.

Además, la composición respeta mejor la encapsulación, ya que no expone detalles internos de las clases utilizadas.

## 12. Herencia vs. Composición. Se dice que la *"herencia rompe la encapsulación"*, ¿a qué se refiere esto?

### Respuesta

La herencia puede romper la encapsulación porque las subclases dependen de detalles internos de la superclase, especialmente de los miembros protegidos. Cambios internos en la clase base pueden afectar directamente al funcionamiento de las subclases.

Aunque estos detalles no sean públicos, forman parte del contrato implícito entre la superclase y sus subclases. Esto reduce la libertad de modificar la implementación sin consecuencias.

La composición evita este problema al interactuar únicamente a través de interfaces bien definidas, manteniendo los detalles internos ocultos.

## 13. Pongamos un ejemplo de dos alternativas para lo mismo. Tenemos un `Estudiante` y un `Trabajador`, ambos tienen datos en común: el DNI y el nombre. Modelemos esto de dos formas: uno por herencia, con una superclase `Persona`, y otro con composición, con una clase `DatosPersonales`. Se debe recibir una instancia de `DatosPersonales` en el constructor de la clase `Estudiante` y `Trabajador`.

### Respuesta

En la solución mediante herencia, se extraen los datos comunes a una superclase `Persona`. Esto expresa que tanto estudiante como trabajador “son una persona”, compartiendo identidad y estado.

En la solución mediante composición, los datos personales se encapsulan en una clase independiente. Estudiante y trabajador no “son” datos personales, sino que los “tienen”, lo que ofrece mayor flexibilidad de diseño.

Ambas soluciones son válidas, pero transmiten significados conceptuales distintos y deben evaluarse según el dominio del problema.

```java
// Herencia
public class Persona {
    private String dni;
    private String nombre;
}

// Composición
public class DatosPersonales {
    private String dni;
    private String nombre;
}

public class Estudiante {
    private DatosPersonales datos;

    public Estudiante(DatosPersonales datos) {
        this.datos = datos;
    }
}

public class Trabajador {
    private DatosPersonales datos;

    public Trabajador(DatosPersonales datos) {
        this.datos = datos;
    }
}
```

```
```
