# TEMA 3. Excepciones

---

## 1. Empecemos un tema sobre control de errores en lenguajes de programación, con algo básico. En C, donde no existen las excepciones, pongamos un ejemplo de una raíz que toma número flotante positivo. Queremos controlar el error si la función recibe un número negativo. El usuario debe ser informado pero desde fuera de la función `raiz` ¿Cómo indicamos ese error? Enumera dos opciones diferentes de diseñar, poniendo un ejemplo de código de cada una.


En C no existe un mecanismo automático para notificar errores como ocurre en lenguajes orientados a objetos. Por ello, una estrategia típica consiste en devolver un **código de error especial** desde la propia función. Este código suele ser un valor reservado que no puede confundirse con un resultado correcto, por ejemplo `-1` en una función que calcula raíces cuadradas. El programa llamador debe revisar explícitamente el valor devuelto para decidir si se produjo un error y actuar en consecuencia.

Una segunda alternativa consiste en usar un **parámetro adicional** pasado por referencia, en el que la función escribe un indicador de error. En este caso, el resultado se devuelve de forma normal, mientras que la variable externa especifica si hubo un problema. Este enfoque separa el resultado del estado de error y evita sobrecargar el valor devuelto con significados adicionales, lo cual resulta más claro en funciones complejas.

**Ejemplo 1: código de retorno**

```c
float raiz(float x) {
    if (x < 0) return -1;
    return sqrt(x);
}
```

**Ejemplo 2: parámetro extra para error**

```c
float raiz(float x, int *error) {
    if (x < 0) { *error = 1; return 0; }
    *error = 0;
    return sqrt(x);
}
```

---

## 2. Brevemente ¿Qué es una **"excepción"**? ¿Con qué objetivo las usa un programador cuando implementa funciones o cuando las llama?


Una excepción es un mecanismo que permite interrumpir el flujo normal de ejecución cuando ocurre una condición anómala. En lugar de devolver un valor especial o usar variables externas, la función que detecta el problema genera una señal que viaja automáticamente hacia arriba hasta encontrar un manejador adecuado. Esto evita mezclar la lógica principal con la lógica de control de errores y facilita la lectura del código.

El programador la utiliza para indicar fallos que no deberían gestionarse en el mismo nivel donde se detectan, sino en un punto más alto de la ejecución. Al llamar a funciones, las excepciones permiten concentrarse en el flujo principal sin preocuparse continuamente por los errores locales, ya que éstos se capturan cuando realmente es necesario tratarlos.

---

## 3. Reescribe el mismo ejemplo de raíz, pero en Java, metiendo ese método en una clase `Calculadora` y llamando a dicho método desde el método `main`, mostrando cómo se puede controlar desde fuera.


En Java, una práctica habitual consiste en encapsular el comportamiento dentro de una clase y emplear excepciones para indicar errores. El método de cálculo lanza una excepción cuando recibe un argumento inválido. De este modo, se separa la lógica del cálculo de la lógica del manejo de errores, quedando esta última en el código llamador.

Al ejecutar el método desde `main`, se envuelve la llamada en un bloque `try-catch`. Si el parámetro es negativo, se lanza una excepción que es capturada en `main`, donde se muestra el mensaje correspondiente. Este enfoque evita tener que devolver valores inválidos y mejora la claridad del código.

```java
class Calculadora {
    public static double raiz(double x) {
        if (x < 0) {
            throw new IllegalArgumentException("Número negativo");
        }
        return Math.sqrt(x);
    }
}

public class Main {
    public static void main(String[] args) {
        try {
            double r = Calculadora.raiz(-9);
            System.out.println("Resultado: " + r);
        } catch (IllegalArgumentException e) {
            System.out.println("Error capturado: " + e.getMessage());
        }
    }
}
```

---

## 4. ¿Qué es **"lanzar"** una excepción? ¿Qué es **"controlar"** o **"capturar"** una excepción? ¿Qué es que se **"propague"** una excepción? ¿Qué le va ocurriendo a las funciones en la pila de llamadas por donde se va propagando la excepción? ¿Las funciones que no la controlan se reanudan después de alguna forma? Explica con el mismo ejemplo anterior en Java de la raíz cuadrada.


"Lanzar" una excepción significa crear un objeto que representa el error y enviarlo hacia el exterior de la función. Esto se hace con la palabra clave `throw`. En el ejemplo de la raíz, cuando se pasa un número negativo, el método lanza una `IllegalArgumentException`. La ejecución dentro del método se detiene inmediatamente y el control se transfiere hacia niveles superiores de la pila de llamadas.

"Capturar" o "controlar" una excepción consiste en interceptarla mediante un bloque `catch` asociado a un `try`. El primer bloque `catch` cuyo tipo coincida con el de la excepción la recibirá y permitirá tomar acciones concretas, como mostrar un mensaje al usuario. Durante la propagación, cada función en la pila finaliza abruptamente sin continuar sus instrucciones pendientes. Ninguna de estas funciones se reanuda después; sólo se ejecutará el código dentro del `catch` donde finalmente se capture la excepción.

---

## 5. ¿Qué ventajas tiene frente a C, la **"propagación natural"** de las excepciones a través de la pila (*stack*) de llamadas?


Una ventaja importante es que no se obliga al programador a comprobar manualmente el estado de error después de cada llamada a una función. En lenguajes sin excepciones, suele ser necesario inspeccionar códigos de retorno continuamente, lo que genera código repetitivo y propenso a ser olvidado. Con las excepciones, cualquier error importante viajará automáticamente por la pila hasta el lugar adecuado para su manejo.

Además, la propagación permite que las decisiones de cómo responder a un error se tomen en un nivel más alto del programa, donde se tiene una visión global del contexto. Esto hace que el diseño sea más modular y que cada función se concentre en su tarea principal. También reduce el acoplamiento entre funciones, ya que cada una no debe conocer cómo se gestionan los errores en niveles superiores.

---

## 6. En orientación a objetos, ¿las excepciones suelen ser objetos? ¿Qué ventajas tiene esto en términos de encapsulación? ¿Podemos entonces crear excepciones personalizadas?


En lenguajes orientados a objetos como Java, las excepciones se representan mediante objetos. Esto permite encapsular en ellos información relevante sobre el error ocurrido, como mensajes detallados, causas internas y trazas de pila. Al tratarse de objetos, pueden manipularse como cualquier otra instancia, manteniendo una estructura coherente con el resto del lenguaje.

Gracias a esta encapsulación, es posible crear excepciones personalizadas extendiendo clases base como `Exception` o `RuntimeException`. Esto facilita definir tipos de error específicos de una aplicación o módulo, permitiendo un manejo más preciso y expresivo. De este modo, cada tipo de error puede contener la información necesaria para comprender su origen y consecuencias.

---

## 7. ¿Qué **información esencial** lleva cualquier **objeto excepción** que es muy útil tener cuando se llega a un manejador?


Normalmente un objeto excepción contiene un mensaje descriptivo que ayuda a identificar qué ha sucedido. Este mensaje puede incluir detalles sobre la causa del error o datos que faciliten su reproducción. Al llegar a un manejador, esta información resulta crucial para mostrar mensajes claros al usuario o para realizar un registro del problema.

Otra pieza de información fundamental es la traza de pila, que muestra la secuencia de llamadas que llevaron al error. Esta traza indica el archivo, clase y línea donde se originó la excepción, así como todas las funciones que la propagaron. Gracias a esto, depurar un fallo se vuelve mucho más sencillo que en lenguajes donde esta información debe gestionarse manualmente.

---

## 8. En Java, sobre el bloque **"try-catch"**, ¿se pueden tener más de un bloque `catch`? ¿cuántos bloques `catch` se ejecutan?


En Java es posible colocar múltiples bloques `catch` debajo de un mismo `try`. Cada uno está pensado para interceptar un tipo concreto de excepción, permitiendo así un tratamiento diferenciado según la naturaleza del error detectado. Este enfoque favorece un manejo más preciso y estructurado de los fallos.

A pesar de que puede haber varios `catch`, únicamente uno de ellos se ejecutará: el primero que coincida con el tipo dinámico de la excepción lanzada. Una vez activado un `catch`, los demás se ignoran, ya que el error ya ha sido manejado. Esto garantiza que no exista ambigüedad en la gestión de la excepción.

---

## 9. Si las excepciones producen rupturas en el código llamador, ¿cómo podemos garantizar que se ejecuta siempre finalmente un código necesario para cierre de ficheros o liberación de recursos? Pon un ejemplo en Java con `finally`, tanto con `catch` como sin él.


Java proporciona el bloque `finally` para asegurar la ejecución de cierto código incluso cuando ocurre una excepción. Este bloque es útil para liberar recursos críticos como archivos abiertos, conexiones o memoria reservada. Independientemente de que el código dentro del `try` termine de forma normal o abrupta, lo que esté en `finally` siempre se ejecuta.

Un `finally` puede acompañar a uno o varios `catch`, o incluso aparecer sin ellos. Cuando se usa junto a un `catch`, el manejo del error puede realizarse primero y acto seguido se ejecuta el código de limpieza. Sin `catch`, el error sigue propagándose después de ejecutar el `finally`, pero se garantiza que los recursos quedan correctamente liberados.

**Con catch:**

```java
try {
    abrirArchivo();
} catch (IOException e) {
    System.out.println("Error: " + e.getMessage());
} finally {
    System.out.println("Cerrando recursos...");
}
```

**Sin catch:**

```java
try {
    abrirArchivo();
} finally {
    System.out.println("Liberando recursos...");
}
```

---

## 10. En Java, el bloque `finally` puede ir sin `catch`? ¿Se ejecuta siempre tanto si ocurre como si no ocurre una excepción? ¿Y si hay un `return` en medio del `try`?


El lenguaje permite que un bloque `finally` esté presente sin necesidad de un bloque `catch`. En este caso, el `finally` se usa para garantizar tareas de limpieza, mientras que la excepción, si la hay, se propagará hacia el llamador una vez ejecutado el código de cierre. Esto muestra que la responsabilidad del manejo puede delegarse sin sacrificar la liberación de recursos.

El código dentro de `finally` se ejecuta en todos los casos: si el `try` concluye correctamente, si lanza una excepción, o incluso si contiene una instrucción `return`. En presencia de un `return`, la ejecución del método se pospone hasta que se termine el bloque `finally`. Este comportamiento asegura que los recursos se liberen de forma consistente y evita fugas accidentales.

---

## 11. En Java, ¿qué son las excepciones **"controladas"** y las **"no controladas"**? ¿Qué papel juega `RuntimeException`? Da ejemplos de cada tipo y listas de situaciones habituales donde se prefiere una u otra.


Las excepciones controladas (checked) son aquellas que el compilador obliga a declarar mediante `throws` o capturar explícitamente con un bloque `catch`. Suelen corresponder a situaciones en las que el error es recuperable, como un archivo no encontrado o un problema de entrada-salida. Entre sus ejemplos típicos se encuentran `IOException`, `FileNotFoundException` y `SQLException`.

Por otro lado, las excepciones no controladas (unchecked) son aquellas que derivan de `RuntimeException`. El compilador no obliga a declararlas ni a capturarlas, pues suelen indicar errores de programación, como argumentos inválidos, accesos fuera de rango o divisiones por cero. Ejemplos comunes incluyen `IllegalArgumentException`, `NullPointerException` y `ArithmeticException`.

**Situaciones donde se prefieren controladas:**

* Operaciones de entrada/salida.
* Acceso a bases de datos.
* Uso de recursos externos.
* Errores que el usuario o el entorno pueden corregir.

**Situaciones donde se prefieren no controladas:**

* Violaciones de contrato de un método.
* Argumentos incorrectos enviados por el programador.
* Estados imposibles en el flujo lógico.
* Errores que indican fallos de programación.

---

## 12. ¿Qué es y para qué se usa `throws`? ¿Por qué es alternativa a capturar una excepción controlada?


La cláusula `throws` se utiliza en la firma de un método para indicar que puede generar una excepción controlada y que no se hace responsable de gestionarla internamente. Esto implica que será el método llamador quien deberá decidir cómo manejar la situación, ya sea capturando la excepción o continuando la propagación hacia niveles superiores. De esta forma, el método se concentra en su funcionalidad principal sin mezclarse con la lógica de manejo de errores.

Es una alternativa a capturar porque permite delegar la responsabilidad del tratamiento de la excepción. En muchos casos, el método no dispone de información suficiente para reaccionar adecuadamente ante un fallo externo, por lo que resulta más razonable dejar la gestión a quien tenga más contexto. Así se mantiene una separación clara entre la lógica de negocio y la lógica de control de errores.

---

## 13. Pon un ejemplo en Java de firma de método que incluya `throws`, de una función que abre un fichero pero que declara que no le interesa manejar la excepción de si el fichero no existe, sino que se propague hacia arriba. Eso sí, acuérdate del `finally`.


Cuando un método abre un fichero, puede producirse un error si el archivo no existe o no se tiene permiso de lectura. Si no se desea gestionar este error en el propio método, se puede declarar mediante `throws` que la excepción se propaga al llamador. Sin embargo, es importante liberar los recursos correctamente, algo que se garantiza mediante el uso de `finally`.

En el ejemplo siguiente se abre un archivo con `FileInputStream`. Si ocurre un `FileNotFoundException`, no se captura dentro de la función, sino que el error se envía hacia arriba. Aun así, el `finally` se asegura de cerrar el archivo en caso de que haya llegado a abrirse, evitando fugas de recursos.

```java
public static void abrir(String nombre) throws FileNotFoundException {
    FileInputStream f = null;
    try {
        f = new FileInputStream(nombre);
        System.out.println("Fichero abierto.");
    } finally {
        if (f != null) {
            try { f.close(); } catch (IOException e) {}
        }
        System.out.println("Finalizando apertura.");
    }
}
```

---

## 14. ¿Podemos poner en `throws` excepciones no controladas, como `RuntimeException`? ¿Debería el método llamador entonces poner `try-catch` en ese caso? ¿Qué sentido tendría?


Es técnicamente posible incluir excepciones no controladas dentro de la cláusula `throws`. No obstante, esto no es necesario porque el compilador no exige declarar ni capturar este tipo de excepciones. Declararlas explícitamente solo sirve como documentación, para que quien utilice el método sepa que puede producirse cierto tipo de error. Pero desde el punto de vista del compilador, no aporta ninguna obligación adicional.

El método llamador no está obligado a usar un `try-catch` frente a una excepción no controlada, incluso cuando aparece en `throws`. El sentido de capturar estas excepciones dependerá de si el código llamador puede ofrecer una recuperación razonable. En general, las no controladas se asocian a errores de programación donde no suele ser adecuado intentar recuperarse.

---

## 15. ¿Cuándo se recomienda usar excepciones controladas, como `IOException`, y cuándo no controladas como `IllegalArgumentException`? ¿Existen en todos los lenguajes ambas opciones? En los que sólo existe una opción, ¿cuál es la más habitual?


Las excepciones controladas se recomiendan cuando el error es previsible y está fuera del control del programador, como en operaciones de entrada/salida, comunicación con sistemas externos o acceso a recursos del sistema. En estos casos, el fallo puede deberse a condiciones externas, como un archivo inexistente o un dispositivo desconectado, y el código tiene posibilidades reales de recuperarse.

Las excepciones no controladas se prefieren cuando se trata de errores de programación, como enviar argumentos inválidos o acceder a elementos fuera de rango. Estos errores indican violaciones de contrato que deberían corregirse durante el desarrollo y no en tiempo de ejecución. No tiene sentido obligar a capturarlos porque, en la mayoría de los casos, su aparición significa que existe un fallo lógico en el código.

No todos los lenguajes disponen de ambos mecanismos. Lenguajes como C++, Python o JavaScript solo usan excepciones no controladas. En estos entornos, el estilo más común es el de excepciones no controladas, que simplifican el manejo y evitan la sobrecarga de declarar cada posible error.

---

## 16. ¿Tiene sentido lanzar excepciones dentro del `catch`? ¿Se puede relanzar la misma excepción capturada? ¿Cuándo tendría sentido hacer esto último? Pon ejemplos de ambos casos.

Lanzar una excepción dentro de un bloque `catch` puede ser útil cuando el manejador detecta que no puede resolver el problema completamente. En estos casos, se genera una nueva excepción más adecuada al nivel de abstracción actual y se envía hacia arriba para que otro componente la trate. Este proceso permite transformar errores de bajo nivel en errores de alto nivel más comprensibles.

También es posible relanzar la misma excepción capturada, simplemente reusando el objeto original. Esta técnica se emplea cuando se desea registrar información adicional o realizar alguna tarea de limpieza, pero sin impedir que el error siga su curso normal. El relanzamiento preserva la causa original y permite que niveles superiores decidan cómo manejar el problema.

**Ejemplo lanzando nueva excepción:**

```java
catch (IOException e) {
    throw new RuntimeException("Fallo leyendo datos", e);
}
```

**Ejemplo relanzando la misma:**

```java
catch (IOException e) {
    System.out.println("No se pudo procesar el archivo");
    throw e;
}
```

---

## 17. ¿En qué consiste que una excepción sea la **"causa"** de otra excepción? Pon un ejemplo en Java, donde capturemos una excepción de bajo nivel y la encapsulemos en otra personalizada de alto nivel. Cuando una excepción sale por pantalla y tiene una causa, ¿se ve?

En Java, una excepción puede contener internamente otra excepción que actuó como causa original del problema. Esto permite preservar la información del error de bajo nivel, a la vez que se ofrece una excepción más significativa al nivel superior de la aplicación. El encadenamiento de excepciones facilita la depuración, ya que mantiene un historial completo de cómo se originó el error.

Cuando se lanza una excepción de alto nivel con una causa, ambas aparecen en la traza de pila mostrada por la máquina virtual. La salida incluye la excepción principal seguida de un bloque "Caused by" que detalla la excepción original. Esto hace posible entender el error tanto en su origen técnico como en su interpretación funcional.

**Ejemplo:**

```java
class ErrorDeAltoNivel extends Exception {
    public ErrorDeAltoNivel(String msg, Throwable cause) {
        super(msg, cause);
    }
}

try {
    abrir("datos.txt");
} catch (FileNotFoundException e) {
    throw new ErrorDeAltoNivel("No se pudo cargar los datos", e);
}
```

