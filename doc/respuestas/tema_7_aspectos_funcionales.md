<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Aspectos funcionales". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: clases y objetos, encapsulación, excepciones, composición, herencia, polimorfismo y genericidad.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->

# TEMA 7. Aspectos funcionales

## 1. ¿Qué es un puntero a una función? Pon un ejemplo de código en C, donde se define una función y que reciba una cadena de caracteres como parámetro y devuelva la cadena en mayúsculas. Crea un puntero en una variable local a dicha función llamado `aMayusculas` e invócala con el puntero.

### Respuesta

Un puntero a una función es una variable que almacena la dirección de memoria de una función en C. De esta forma, es posible pasar funciones como parámetros a otras funciones o almacenarlas en estructuras de datos. La sintaxis para declarar un puntero a función es: `tipoRetorno (*nombrePuntero)(tiposParametros)`.

En el siguiente ejemplo se define una función que convierte una cadena a mayúsculas y se crea un puntero a esa función:

```c
#include <stdio.h>
#include <ctype.h>
#include <string.h>

char* toUpperCase(char* str) {
    for (int i = 0; str[i] != '\0'; i++) {
        str[i] = toupper(str[i]);
    }
    return str;
}

int main() {
    char* (*aMayusculas)(char*) = toUpperCase;
    
    char cadena[] = "hola mundo";
    printf("%s\n", aMayusculas(cadena));
    
    return 0;
}
```

El puntero `aMayusculas` se invoca de la misma forma que la función original, utilizando la sintaxis `aMayusculas(cadena)`. Esta capacidad de los punteros a funciones es fundamental en programación en C y sirve como base para entender conceptos más avanzados como las funciones de orden superior.


## 2. ¿Qué es una **función lambda** en un lenguaje de programación? Pon un ejemplo similar al anterior en Javascript y otro en Java con funciones lambda. Usa una variable local `aMayusculas` para apuntar a la función lambda. Por simplicidad, en Java, emplea `Function<String, String>` para el tipo de la referencia a la función lambda.

### Respuesta

Una función lambda es una función anónima y sin nombre que permite escribir código funcional de forma concisa. A diferencia de los punteros a funciones en C, las funciones lambda tienen una sintaxis más limpia y están integradas en el lenguaje como constructos de primera clase. Una función lambda puede capturar variables del contexto donde se define, lo que permite acceder a variables locales.

A continuación se muestran ejemplos en JavaScript y Java:

```javascript
const aMayusculas = (str) => str.toUpperCase();
console.log(aMayusculas("hola mundo"));
```

```java
Function<String, String> aMayusculas = str -> str.toUpperCase();
System.out.println(aMayusculas.apply("hola mundo"));
```

En JavaScript, la función lambda se define con la sintaxis de flecha `(parámetros) => cuerpo`. En Java, la sintaxis es `parámetros -> cuerpo`, donde `Function<String, String>` indica que recibe un `String` y devuelve un `String`. La invocación en Java se realiza mediante el método `apply()` que proporciona la interfaz `Function`.


## 3. ¿Qué es el **paradigma funcional**? ¿Por qué a algunos lenguajes orientados a objetos como Java 8, se les llama multi-paradigma? ¿Qué quiere decir que las funciones son "ciudadanos de primera clase"?

### Respuesta

El paradigma funcional es un estilo de programación que enfatiza el uso de funciones como unidades computacionales fundamentales. Se basa en conceptos matemáticos como las funciones puras (sin efectos secundarios) y la composición de funciones. A diferencia del enfoque imperativo, el paradigma funcional evita el cambio de estado y favorece la construcción de soluciones mediante la combinación de funciones simples.

Java 8 se considera un lenguaje multi-paradigma porque combina características de la programación orientada a objetos con características de la programación funcional. Antes de Java 8, Java era principalmente orientado a objetos, pero con la introducción de las funciones lambda, los streams y las referencias a métodos, se agregó soporte para el paradigma funcional sin abandonar la orientación a objetos.

Decir que las funciones son "ciudadanos de primera clase" significa que pueden ser tratadas como datos: se pueden asignar a variables, pasar como parámetros a otras funciones, devolver como resultado de funciones, y almacenar en estructuras de datos. En lenguajes sin funciones como ciudadanos de primera clase (como C sin punteros a funciones), las funciones son más rígidas y no pueden manipularse de la misma forma.


## 4. Explica la sintaxis básica de una función lambda en Java.

### Respuesta

La sintaxis básica de una función lambda en Java es: `(parámetros) -> cuerpo`. Los parámetros se encierran entre paréntesis, se utiliza el operador `->` como separador, y el cuerpo contiene la lógica de la función.

Existen varias variantes según el número de parámetros y la complejidad del cuerpo:

```java
// Sin parámetros
() -> System.out.println("Hola");

// Un parámetro (se pueden omitir paréntesis)
str -> str.toUpperCase();
// o con paréntesis
(str) -> str.toUpperCase();

// Múltiples parámetros
(a, b) -> a + b;

// Cuerpo con múltiples sentencias (entre llaves)
(a, b) -> {
    int resultado = a + b;
    return resultado;
};

// Cuerpo con una única expresión (el return es implícito)
(a, b) -> a + b;
```

Cuando el cuerpo contiene una única expresión, esta se evalúa y devuelve automáticamente. Si se necesitan múltiples sentencias, se utilizan llaves y debe incluirse explícitamente la palabra clave `return`. El compilador de Java deduce automáticamente los tipos de los parámetros basándose en el contexto donde se utiliza la lambda.


## 5. Ahora recibamos una función como parámetro a un método y la llamaremos desde dentro. Amplia los ejemplos anteriores de Java y JavaScript con un método llamado `transformar`, que reciba un `String` como parámetro y luego una función transformadora como lo es `aMayúsculas` y la invoque desde dentro.

### Respuesta

Las funciones de orden superior son funciones que reciben otras funciones como parámetros o devuelven funciones. En JavaScript:

```javascript
const transformar = (str, funcion) => funcion(str);
const aMayusculas = (str) => str.toUpperCase();
console.log(transformar("hola", aMayusculas));
```

En Java, el método `transformar` recibe un `String` y una función transformadora encapsulada en una interfaz funcional:

```java
import java.util.function.Function;

public static String transformar(String str, Function<String, String> transformador) {
    return transformador.apply(str);
}

public static void main(String[] args) {
    Function<String, String> aMayusculas = str -> str.toUpperCase();
    System.out.println(transformar("hola", aMayusculas));
}
```

En ambos lenguajes, el método `transformar` recibe la función como parámetro y la invoca sobre la cadena. Esta es la base de la programación funcional: permitir que el comportamiento de una función sea determinado por otra función que se pasa como argumento, lo que aumenta significativamente la flexibilidad y reutilización del código.


## 6. Ahora, invoca `transformar`, con una nueva función lambda directamente en la llamada a `transformar`, por ejemplo, una función lambda que invierta la cadena. Define la función de inversión justo cuando la estás pasando como parámetro.

### Respuesta

Es posible definir funciones lambda directamente en el lugar donde se necesitan, sin necesidad de crear una variable intermedia. Esto es especialmente útil para funciones simples que se utilizan una sola vez.

En JavaScript:

```javascript
const transformar = (str, funcion) => funcion(str);
console.log(transformar("hola", (str) => str.split('').reverse().join('')));
```

En Java:

```java
System.out.println(transformar("hola", 
    str -> new StringBuilder(str).reverse().toString()));
```

Esta práctica mejora la legibilidad cuando la función es simple y su propósito está claro en el contexto. Las funciones lambda anónimas proporcionan una sintaxis muy concisa que evita la creación de clases o variables innecesarias, haciendo el código más expresivo y enfocado en la lógica principal.


## 7. ¿Qué se entiende por cierre o "closure" en el contexto de las funciones lambda? Pon un ejemplo en Java de cómo una función lambda es capaz de acceder a una variable local en el contexto donde fue definida. Modifica el ejemplo anterior, creando otra función lambda para transformar una cadena, pero que lo que haga es concatenar a la cadena de entrada otra cadena que está en una variable local definida fuera de la función lambda.

### Respuesta

Un closure es la capacidad de una función lambda de acceder y utilizar variables del contexto donde fue definida, incluyendo variables locales del método que la contiene. A diferencia de los punteros a funciones en C, que no pueden acceder al contexto local, las lambdas capturan automáticamente las variables necesarias. En Java, las variables capturadas deben ser efectivamente finales (no cambiar su valor después de ser utilizadas en la lambda).

A continuación se muestra un ejemplo donde la función lambda accede a una variable local:

```java
String sufijo = " transformado";
Function<String, String> concatenar = str -> str + sufijo;
System.out.println(transformar("hola", concatenar));
```

En este caso, la lambda `concatenar` captura la variable local `sufijo` y la utiliza en su cuerpo. El closure permite que la función lambda mantenga una referencia a esta variable incluso después de que el método que la definió haya terminado su ejecución. Esta característica es muy poderosa para crear funciones especializadas que dependen de datos del contexto local, algo imposible de realizar directamente con los punteros a funciones simples de C.


## 8. Reflexiona: ¿en qué se diferencia entonces una función lambda de los punteros a funciones que hay en C?

### Respuesta

Las funciones lambda en lenguajes como Java y JavaScript difieren significativamente de los punteros a funciones en C. En primer lugar, las lambdas tienen una sintaxis mucho más clara y concisa, permitiendo definir funciones de forma anónima directamente donde se necesitan. En C, los punteros a funciones requieren una sintaxis compleja y siempre necesitan una función nombrada preexistente a la cual apuntar.

La diferencia más importante es la capacidad de captura de contexto. Las lambdas pueden acceder a variables locales del contexto donde fueron definidas (closures), mientras que los punteros a funciones en C solo pueden acceder a variables globales o a través de parámetros explícitos. Esta capacidad de captura hace que las lambdas sean mucho más flexibles para crear funciones especializadas que dependan de datos locales.

Además, en Java las lambdas están integradas con las interfaces funcionales y el sistema de tipos, proporcionando verificación en tiempo de compilación. Los punteros a funciones en C no tienen esta integración y requieren mayor cuidado manual para evitar errores. En términos modernos, las lambdas son una evolución natural de los punteros a funciones, proporcionando una abstracción más segura, legible y potente para trabajar con funciones como datos.


## 9. Devolvamos ahora funciones. Creemos ahora una función que sea capaz de crear funciones "descuento". Una función "descuento", decrementa un porcentaje pasado como parámetro. Por simplicidad, usa `Function<Double, Double>` para su tipo. La función `crearDescuento(porcentaje)`, recibe solo el porcentaje de descuento a aplicar y devuelve la función de descuento. Prueba a crear dos descuentos distintos y aplicarlos a una cantidad. Explica la closure en la función descuento.

### Respuesta

Una función que devuelve funciones permite crear funciones especializadas basadas en parámetros. En este caso, `crearDescuento` recibe un porcentaje y devuelve una función que aplica ese descuento a cualquier cantidad:

```java
public static Function<Double, Double> crearDescuento(double porcentaje) {
    return cantidad -> cantidad * (1 - porcentaje / 100);
}

public static void main(String[] args) {
    Function<Double, Double> descuento10 = crearDescuento(10);
    Function<Double, Double> descuento20 = crearDescuento(20);
    
    System.out.println(descuento10.apply(100.0)); // 90.0
    System.out.println(descuento20.apply(100.0)); // 80.0
}
```

La closure en este ejemplo ocurre porque la función lambda devuelta captura la variable `porcentaje` del parámetro del método. Cada vez que se crea un descuento con `crearDescuento`, se crea una nueva función lambda que "recuerda" el valor específico del porcentaje que se pasó en ese momento. Esto permite tener múltiples funciones de descuento diferentes, cada una con su propio porcentaje capturado. Sin closures, sería necesario pasar el porcentaje cada vez que se aplique el descuento, complicando significativamente el código.


## 10. En Java, que es un lenguaje con comprobación estática de tipos, donde los tipos se declaran, toda función lambda tiene un tipo, que se conoce como **interfaz funcional**. ¿Qué es una **interfaz funcional**? ¿Qué requisitos tiene?

### Respuesta

Una interfaz funcional en Java es una interfaz que define exactamente un método abstracto (único método sin implementación). Este método abstracto determina la firma de la función lambda que implementa la interfaz. Las interfaces funcionales permiten que las lambdas sean compatibles con el sistema de tipos de Java, proporcionando un contrato claro sobre qué parámetros recibe y qué devuelve la función.

Los requisitos para que una interfaz sea funcional son: debe ser una interfaz, debe tener exactamente un método abstracto, puede tener métodos estáticos o métodos por defecto (default), y puede heredar de otras interfaces siempre que no agreguen nuevos métodos abstractos. Aunque es opcional, se recomienda anotar las interfaces funcionales con `@FunctionalInterface` para que el compilador verifique que cumplan con estos requisitos y para mejorar la legibilidad del código.

Por ejemplo, `Function<T, R>` es una interfaz funcional porque define un único método abstracto `R apply(T t)`. Las interfaces funcionales establecen un contrato entre el código que proporciona la lambda y el código que la utiliza, garantizando que ambos acuerden en los tipos de parámetros y retorno.


## 11. Creemos una interfaz funcional a mano. Por ejemplo, define la interfaz funcional del ejemplo que transforma la cadena en otra. Llámale `Transformador`, que define una función que convierte una cadena de texto (`String`) en otra (`String`).

### Respuesta

Crear una interfaz funcional personalizada es sencillo: solo se necesita definir una interfaz con exactamente un método abstracto que represente la firma de la función deseada.

```java
@FunctionalInterface
public interface Transformador {
    String transformar(String str);
}
```

Una vez definida la interfaz, se puede utilizar con funciones lambda:

```java
Transformador aMayusculas = str -> str.toUpperCase();
Transformador inversa = str -> new StringBuilder(str).reverse().toString();

System.out.println(aMayusculas.transformar("hola")); // HOLA
System.out.println(inversa.transformar("hola"));     // aloH
```

La anotación `@FunctionalInterface` indica explícitamente que la interfaz es funcional y ayuda al compilador a detectar errores si se agreguen métodos adicionales accidentalmente. Esta interfaz personalizada es equivalente a `Function<String, String>`, pero proporciona un nombre más significativo que representa mejor el dominio del problema.


## 12. Ahora hagamos la interfaz funcional algo más genérica y empleando generics, para que permita definir un `Transformador` de un tipo en otro. Pon un ejemplo de un transformador que redondea un `Double` en un `Integer`.

### Respuesta

Al utilizar generics, la interfaz `Transformador` se vuelve reutilizable para transformar de cualquier tipo a cualquier otro tipo:

```java
@FunctionalInterface
public interface Transformador<T, R> {
    R transformar(T entrada);
}
```

Con esta interfaz genérica se pueden crear transformadores de diferentes tipos:

```java
Transformador<String, String> aMayusculas = str -> str.toUpperCase();
Transformador<Double, Integer> redondear = d -> Math.round(d).intValue();
Transformador<String, Integer> longitud = str -> str.length();

System.out.println(aMayusculas.transformar("hola"));     // HOLA
System.out.println(redondear.transformar(3.7));          // 4
System.out.println(longitud.transformar("java"));        // 4
```

La interfaz genérica `Transformador<T, R>` define que el método `transformar` recibe un parámetro de tipo `T` y devuelve un valor de tipo `R`. Esto proporciona mayor flexibilidad manteniendo la seguridad de tipos en tiempo de compilación. De esta manera, una única definición de interfaz puede usarse para transformaciones entre cualquier combinación de tipos.


## 13. `Transformador`, en su versión genérica, parece muy útil y reutilizable, hasta el punto de que es igual a una interfaz funcional que ya hay, que es `Function<T, R>`. Muestra las interfaces funcionales predefinidas que hay en Java.

### Respuesta

Java proporciona un conjunto de interfaces funcionales predefinidas en el paquete `java.util.function` que cubren los casos de uso más comunes. Las principales son:

- **`Function<T, R>`**: Recibe un parámetro de tipo `T` y devuelve un resultado de tipo `R`. Equivalent a `Transformador<T, R>`.
- **`Consumer<T>`**: Recibe un parámetro de tipo `T` y no devuelve nada (void). Útil para operaciones con efectos secundarios.
- **`Supplier<T>`**: No recibe parámetros y devuelve un valor de tipo `T`.
- **`Predicate<T>`**: Recibe un parámetro de tipo `T` y devuelve un `boolean`. Útil para filtros y condiciones.
- **`BiFunction<T, U, R>`**: Recibe dos parámetros y devuelve un resultado.
- **`BiConsumer<T, U>`**: Recibe dos parámetros y no devuelve nada.
- **`UnaryOperator<T>`**: Recibe un parámetro de tipo `T` y devuelve un resultado del mismo tipo `T`.
- **`BinaryOperator<T>`**: Recibe dos parámetros de tipo `T` y devuelve un resultado de tipo `T`.

Estas interfaces reutilizables están optimizadas por el compilador y son la base de la programación funcional en Java, reduciendo la necesidad de crear interfaces funcionales personalizadas.


## 14. Vamos a ver ejemplos expresivos de funcional en Java. Estudiemos el `List.forEach`, como versión funcional del bucle `for`. Emplea el `forEach` para recorrer una lista de `Integer` y que muestre un mensaje si el entero es positivo.

### Respuesta

El método `forEach` en las colecciones de Java es una forma funcional de iterar sobre elementos. Es más expresivo que un bucle `for` tradicional porque permite enfocarse en la operación a realizar en lugar de la mecánica de la iteración.

```java
List<Integer> numeros = Arrays.asList(-3, 5, -1, 8, 2, -4);

numeros.forEach(num -> {
    if (num > 0) {
        System.out.println(num + " es positivo");
    }
});
```

Alternatively, se puede utilizar `stream()` con `filter` para una versión más funcional:

```java
numeros.stream()
    .filter(num -> num > 0)
    .forEach(num -> System.out.println(num + " es positivo"));
```

El método `forEach` recibe un `Consumer<Integer>` (una función que recibe un entero y no devuelve nada). Esta aproximación funcional es más concisa y declarativa que escribir un bucle `for` tradicional, porque expresa claramente qué acción se realiza con cada elemento sin necesidad de gestionar índices o iteradores manualmente.

## 15. Repasando el tema de genericidad, fíjate en la firma de `forEach`, ¿por qué se usa `Consumer<? super T>` y no `Consumer<T>`? Explica qué significa **PECS**, y explícalo para el caso de mejorar el ejemplo del método `transformar` la hora de definir el tipo de la función transformadora.

### Respuesta

La firma de `forEach` utiliza `Consumer<? super T>` en lugar de `Consumer<T>` para mayor flexibilidad. El wildcard `? super T` significa que se acepta un `Consumer` que pueda consumir `T` o cualquier superclase de `T`. Esto es importante porque permite pasar un `Consumer<Object>` a un método que trabaja con cualquier tipo específico, aumentando la reutilización del código.

PECS significa "Producer Extends, Consumer Super" (Productor Extiende, Consumidor Super). Esta regla indica cuándo usar wildcards en genéricos: utilizar `extends` cuando el genérico produce (devuelve) valores, y `super` cuando consume (recibe) valores. Aplicando PECS al método `transformar`, si se quiere que acepte funciones más genéricas:

```java
public static <T, R> R transformar(T entrada, Function<? super T, ? extends R> transformador) {
    return transformador.apply(entrada);
}
```

Esto permite pasar funciones que acepten superclases de `T` o devuelvan subclases de `R`, haciendo el método más flexible. Sin PECS, se estaría limitado a exactamente `Function<T, R>`, lo que reduciría la reutilización. Esta es una técnica avanzada de genericidad que mejora significativamente la flexibilidad de las APIs funcionales.

## 16. Referencias a métodos. Podemos obtener una referencia a métodos de objetos o clases. Pon un ejemplo en JavaScript y en Java, de una clase `Persona` con un método `saludar`. En el código principal, crea una `Persona` con un nombre, y obtén una referencia a su método `saludar` en una variable local. Invoca `saludar` con esa referencia a su método `saludar`.

### Respuesta

Una referencia a método permite obtener una referencia a un método sin invocarlo inmediatamente. En JavaScript, esto se logra asignando el método a una variable:

```javascript
class Persona {
    constructor(nombre) {
        this.nombre = nombre;
    }
    saludar() {
        return "Hola, soy " + this.nombre;
    }
}

const persona = new Persona("Juan");
const saludar = persona.saludar;
console.log(saludar());  // Error: 'this' no está vinculado
// Correcto: usar bind
const saludar = persona.saludar.bind(persona);
console.log(saludar());
```

En Java, la sintaxis es `objeto::metodo`:

```java
public class Persona {
    private String nombre;
    public Persona(String nombre) { this.nombre = nombre; }
    public String saludar() { return "Hola, soy " + nombre; }
}

Persona persona = new Persona("Juan");
Supplier<String> saludar = persona::saludar;
System.out.println(saludar.get());
```

Las referencias a métodos en Java utilizan la sintaxis de doble dos puntos `::` y se asignan a interfaces funcionales apropiadas. En este caso, `Supplier<String>` porque el método no recibe parámetros y devuelve un `String`. Las referencias a métodos proporcionan una sintaxis más clara que las lambdas cuando simplemente se quiere delegar el trabajo a un método existente.


## 17. ¿Qué tipos de referencias a método se pueden hacer en Java? Pon un ejemplo de referencia a método estático, a constructor, a método de instancia de una instancia concreta y a método de instancia sobre cualquier instancia.

### Respuesta

En Java hay cuatro tipos de referencias a métodos, cada una con una sintaxis y un propósito diferente:

```java
public class Persona {
    private String nombre;
    public Persona(String nombre) { this.nombre = nombre; }
    public String saludar() { return "Hola, " + nombre; }
    public static void despedir() { System.out.println("Adiós"); }
}

// 1. Referencia a método estático: Clase::metodoEstatico
Consumer<Void> despedir = v -> Persona.despedir();
// o mejor con referencia:
Runnable despedida = Persona::despedir;

// 2. Referencia a constructor: Clase::new
Function<String, Persona> constructor = Persona::new;
Persona p1 = constructor.apply("Ana");

// 3. Referencia a método de instancia de una instancia concreta: instancia::metodo
Persona persona = new Persona("Juan");
Supplier<String> saludar = persona::saludar;

// 4. Referencia a método de instancia sobre cualquier instancia: Clase::metodo
Function<Persona, String> metodoGenerico = Persona::saludar;
String saludo = metodoGenerico.apply(persona);
```

La diferencia clave entre el tercero y el cuarto tipo es que el primero vincula el método a una instancia específica, mientras que el cuarto crea una función que recibe una instancia como parámetro. El cuarto tipo es más reutilizable porque puede aplicarse a cualquier instancia de la clase.


## 18. Otro ejemplo expresivo. Ordena una lista de `Persona`, cada persona tiene un nombre y una edad (de tipo entero). Ordena la lista de `Persona` con `Collections.sort`, pasándole como comparador una expresión lambda que compare la edad de ambas personas y si tienen la misma edad, se ordene por orden alfabético del nombre. Crea dos versiones: Una con la función de comparación hecha manualmente, y otra empleando `Comparator`.

### Respuesta

Primero, se define la clase `Persona`:

```java
public class Persona {
    private String nombre;
    private int edad;
    public Persona(String nombre, int edad) {
        this.nombre = nombre;
        this.edad = edad;
    }
    public String getNombre() { return nombre; }
    public int getEdad() { return edad; }
    public String toString() { return nombre + " (" + edad + ")"; }
}
```

Versión con lambda manual:

```java
List<Persona> personas = Arrays.asList(
    new Persona("Ana", 30),
    new Persona("Carlos", 25),
    new Persona("Beatriz", 25)
);

Collections.sort(personas, (p1, p2) -> {
    if (p1.getEdad() != p2.getEdad()) {
        return Integer.compare(p1.getEdad(), p2.getEdad());
    }
    return p1.getNombre().compareTo(p2.getNombre());
});

personas.forEach(System.out::println);
```

Versión empleando métodos de `Comparator`:

```java
Collections.sort(personas, 
    Comparator.comparingInt(Persona::getEdad)
        .thenComparing(Persona::getNombre));
personas.forEach(System.out::println);
```

La segunda versión es más clara y aprovecha los métodos estáticos de `Comparator` como `comparingInt` y `thenComparing`, que permiten encadenar criterios de comparación. Ambas producen el mismo resultado: ordenan primero por edad (ascendente) y luego por nombre (alfabético) para personas con la misma edad.
