<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Clases y Objetos". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: ninguno.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->

# TEMA 1. Clases y objetos

## 1. ¿Cuáles son las cuatro características básicas de la programación orientada a objetos? Describe brevemente cada una

1. Abstracción
Consiste en modelar objetos del mundo real destacando solo los detalles esenciales. Permite simplificar la complejidad ocultando información irrelevante y mostrando solo lo necesario para usar el objeto.
2. Encapsulamiento
Agrupa datos y métodos dentro de una misma entidad (clase) y restringe el acceso a algunos de sus componentes. Esto protege la información y evita modificaciones indebidas mediante niveles de acceso (public, private, protected).
3. Herencia
Permite crear nuevas clases basadas en otras ya existentes, reutilizando código y añadiendo o modificando funcionalidades. Facilita la extensión y mantenimiento del software.
4. Polimorfismo
Posibilita que distintos objetos respondan de manera diferente a un mismo mensaje o método. Esto permite escribir código más flexible, pudiendo usar una misma interfaz con múltiples comportamientos.

## 2. Cita cuatro lenguajes populares que permitan la programación orientada a objetos

Lenguajes populares que permiten la programación orientada a objetos incluyen **Java**, uno de los más utilizados en entornos empresariales, aplicaciones móviles (especialmente Android) y sistemas distribuidos. Su enfoque totalmente orientado a objetos, junto con su máquina virtual (JVM), lo hace muy portable y robusto. Además, su amplia comunidad y ecosistema de librerías facilitan el desarrollo de proyectos de todo tipo.

Otro lenguaje relevante es **C++**, que combina programación orientada a objetos con programación de bajo nivel. Esto permite crear software de alto rendimiento, como motores de videojuegos, sistemas operativos y aplicaciones donde el control sobre la memoria es fundamental. Su flexibilidad lo convierte en un estándar en ingeniería informática.

**Python** también soporta la programación orientada a objetos de forma sencilla y accesible. Aunque permite otros paradigmas, su modelo de clases y objetos es intuitivo y muy utilizado tanto en desarrollo web como en ciencia de datos, automatización y aplicaciones educativas. Su sintaxis clara ayuda a aprender los conceptos fundamentales de la POO.

Por último, **C#** es un lenguaje moderno diseñado por Microsoft con fuerte enfoque en la orientación a objetos. Se usa ampliamente en aplicaciones de escritorio, desarrollo web con .NET y videojuegos mediante Unity. Su integración con el ecosistema de Microsoft y sus características avanzadas lo hacen muy versátil y sólido.

## 3. Los paradigmas anteriores a la POO, ¿Qué es la **programación estructurada**? y, todavía mejor, ¿Qué es la **programación modular**?

La **programación estructurada** es un paradigma que surgió para mejorar la claridad y calidad del código frente a prácticas anteriores como el uso excesivo de saltos (*goto*). Se basa en dividir el programa en bloques lógicos que utilizan tres estructuras fundamentales: secuencia, selección (condicionales) e iteración (bucles). Esto permite que el flujo del programa sea más predecible, legible y fácil de depurar, favoreciendo un razonamiento más ordenado sobre cómo se ejecutan las instrucciones.

Por otro lado, la **programación modular** lleva esta idea un paso más allá al promover la división de un programa en unidades independientes llamadas módulos. Cada módulo agrupa funciones relacionadas y puede desarrollarse, probarse y mantenerse de forma relativamente autónoma. Este enfoque mejora la reutilización de código, reduce la complejidad global del sistema y permite que varios desarrolladores trabajen en distintas partes del proyecto sin interferencias directas.

Ambos paradigmas fueron fundamentales para preparar el camino hacia la Programación Orientada a Objetos, ya que introdujeron la importancia de la organización del código, la separación por responsabilidades y la reducción de la complejidad. Aunque hoy la POO sea predominante, estos conceptos siguen siendo esenciales y se utilizan constantemente incluso dentro de enfoques más modernos.

## 4. ¿Qué tres elementos definen a un objeto en programación orientada a objetos?

En programación orientada a objetos, un objeto se define principalmente por **tres elementos fundamentales**: **atributos**, **métodos** e **identidad**. Estos conceptos permiten modelar entidades del mundo real dentro del software de forma coherente y organizada. Los atributos representan las características o datos que posee un objeto, como el color, el nombre o una cantidad concreta. Estos valores pueden cambiar durante la ejecución del programa, lo que permite que cada objeto tenga su propio estado.

Los **métodos** son las acciones o comportamientos que un objeto puede realizar. Funcionan como funciones asociadas a la clase, y permiten que el objeto interactúe con otros elementos del sistema, modifique su estado o realice cálculos. Gracias a ellos, los objetos no solo almacenan información, sino que también actúan y responden a mensajes.

Por último, la **identidad** es lo que distingue a un objeto de todos los demás, incluso si tienen los mismos atributos y métodos. Dos objetos pueden tener estados idénticos, pero seguir siendo entidades distintas dentro de la memoria del programa. Esta identidad única es crucial en la POO porque permite gestionar múltiples instancias de una misma clase sin confusión.

## 5. ¿Qué es una clase? ¿Es lo mismo que un objeto? ¿Qué es una instancia? ¿Todos los lenguajes orientados a objetos manejan el concepto de clase?

Una **clase** es un molde o plantilla que define cómo serán los objetos: especifica sus atributos (datos) y sus métodos (comportamientos). Es una abstracción que describe un tipo de entidad, pero por sí sola no ocupa memoria ni existe dentro del programa como algo concreto. Solo establece las reglas y estructura a partir de la cual se pueden crear objetos reales.

Un **objeto**, en cambio, es una entidad concreta creada a partir de una clase. Tiene valores propios en sus atributos y puede ejecutar los métodos definidos por la clase. Aunque dos objetos provengan de la misma clase, cada uno mantiene su propio estado, lo que les confiere identidad independiente dentro del programa.

El término **instancia** se refiere precisamente a un objeto creado desde una clase. Instanciar una clase significa construir un ejemplar real de ella en memoria, listo para usarse. Por ejemplo, si “Coche” es una clase, entonces “miCoche” y “tuCoche” son instancias distintas con sus propios datos.

No todos los lenguajes orientados a objetos utilizan el concepto de clase. Lenguajes como **Java, C++, C# o Python** sí lo hacen, pero otros como **JavaScript** originalmente usaban un modelo basado en **prototipos**, donde no existen clases en el sentido tradicional, aunque la sintaxis moderna simule su presencia. Esto demuestra que la orientación a objetos puede implementarse de diversas formas según el lenguaje.

## 6. ¿Dónde se almacenan en memoria los objetos? ¿Es igual en todos los lenguajes? ¿Qué es la **recolección de basura**? 

En la mayoría de lenguajes orientados a objetos, los objetos suelen almacenarse en **la memoria dinámica (heap)**, que permite reservar espacio en tiempo de ejecución. El heap es flexible y permite crear y destruir objetos según las necesidades del programa. Sin embargo, algunos lenguajes también pueden ubicar ciertos objetos o valores asociados en **la pila (stack)** si son de tipo estático o si su ciclo de vida está muy bien definido, aunque esto es menos común para objetos completos en POO.

No todos los lenguajes gestionan la memoria de la misma manera. Por ejemplo, en **C++** el programador decide si un objeto vive en la pila o en el heap, lo que da más control pero también más responsabilidad para liberar la memoria. En cambio, lenguajes como **Java, C# o Python** usan principalmente el heap y delegan la administración de memoria al propio entorno, reduciendo errores relacionados con la gestión manual.

La **recolección de basura (garbage collection)** es un mecanismo automático que libera memoria ocupada por objetos que ya no están en uso. El sistema analiza qué objetos son inaccesibles y elimina su espacio del heap, evitando fugas de memoria y mejorando la estabilidad del programa. Gracias a este proceso, lenguajes modernos pueden manejar la memoria de forma más segura y eficiente sin intervención directa del programador.

Este sistema, sin embargo, no existe en todos los lenguajes. Por ejemplo, **C++ no incorpora un recolector de basura**, lo que obliga al desarrollador a gestionar la memoria manualmente. En lenguajes con GC, se sacrifica parte del control a cambio de seguridad y comodidad, lo que resulta especialmente útil en proyectos grandes o con estructuras dinámicas.

## 7. ¿Qué es un método? ¿Qué es la **sobrecarga de métodos**? 

Un **método** es una función asociada a una clase u objeto que define un comportamiento. Representa las acciones que ese objeto puede realizar, como modificar sus atributos, interactuar con otros objetos o ejecutar cálculos internos. En la POO, los métodos son esenciales porque permiten que los objetos no solo almacenen datos, sino que “se comporten” de forma coherente con el modelo que representan.

La **sobrecarga de métodos** consiste en definir varios métodos con el mismo nombre dentro de una clase, pero con diferentes parámetros (ya sea en número, tipo o ambos). Esto permite que un mismo comportamiento se adapte a distintos tipos de datos o situaciones, mejorando la flexibilidad y la legibilidad del código. El compilador distingue qué versión usar según los argumentos proporcionados en cada llamada.

En lenguajes como C++, Java o C#, la sobrecarga está muy extendida y forma parte natural del diseño de clases. Esta característica facilita el uso intuitivo de métodos con nombres coherentes, sin obligar al programador a inventar variantes como *calcular1*, *calcular2*, etc. Sin embargo, no todos los lenguajes gestionan la sobrecarga de la misma manera: por ejemplo, Python la simula mediante argumentos opcionales y valores por defecto, ya que no la soporta de forma estricta a nivel de compilador.

## 8. Ejemplo mínimo de clase en Java, que se llame Punto, con dos atributos, x e y, con un método que se llame `calculaDistanciaAOrigen`, que calcule la distancia a la posición 0,0. Por sencillez, los atributos deben tener visibilidad por defecto. Crea además un ejemplo de uso con una instancia y uso del método

class Punto {
    double x;  // visibilidad por defecto, coordenada1
    double y;  // visibilidad por defecto, coordenada2

    Punto(double x, double y) {  //define punto con ambas coordenadas
        this.x = x;
        this.y = y;
    }

    double calculaDistanciaAOrigen() {   //define funcion para calcular el modulo(distancia)
        return Math.sqrt(x * x + y * y);
    }
}

public class Main {
    public static void main(String[] args) {    //crea variable p de clase punto, luego define la variable de como distancia llamando a la funcion distancia y por ultimo muestra por pantalla
        Punto p = new Punto(3, 4);
        double d = p.calculaDistanciaAOrigen();
        System.out.println("Distancia al origen: " + d); // 5.0
    }
}

## 9. ¿Cuál es el punto de entrada en un programa en Java? ¿Qué es `static` y para qué vale? ¿Sólo se emplea para ese método `main`? ¿Para qué se combina con `final`?

En Java, el **punto de entrada** de una aplicación es el método `public static void main(String[] args)` (también válido con `String... args`). La JVM busca exactamente ese método en la clase que ejecutas para iniciar el programa; no devuelve valor y recibe los argumentos de línea de comandos. Puede haber varias clases con `main`, pero solo se ejecuta el de la clase que indiques al lanzar `java MiClase`.

`static` indica que un miembro **pertenece a la clase** y no a las instancias: puedes usar `MiClase.metodoEstatico()` sin crear objetos. Se emplea mucho más allá de `main`: en **métodos utilitarios** (por ejemplo, `Collections.sort`), **campos de clase** compartidos por todas las instancias, **bloques estáticos** para inicialización y **clases anidadas estáticas**. También afecta a *imports* estáticos y a patrones como *singleton* (p.ej., `getInstance()` estático).

Combinar `static` con `final` se usa para **constantes**: `public static final double PI = 3.14159;` (visibles en toda la app, únicas por clase e inmutables). Recuerda que `final` significa “no reasignable” para variables, “no sobreescribible” para métodos y “no heredable” para clases; junto con `static`, facilita definir constantes y evitar modificaciones accidentales, además de expresar **inmutabilidad y estabilidad** en la API.

## 10. Intenta ejecutar un poco de Java de forma básica, con los comandos `javac` y `java`. ¿Cómo podemos compilar el programa y ejecutarlo desde linea de comandos? ¿Java es compilado? ¿Qué es la **máquina virtual**? ¿Qué es el *byte-code* y los ficheros `.class`?

Para compilar y ejecutar desde línea de comandos: guarda el código en `Main.java` con una clase pública `Main` que tenga `public static void main(String[] args)`. Luego compila con `javac Main.java`, lo que generará `Main.class`. Ejecuta con `java Main`. Si usas paquetes, respeta la estructura de directorios (por ejemplo, `src/com/ejemplo/Main.java` con `package com.ejemplo;`), compila con `javac -d out src/com/ejemplo/Main.java` y ejecuta con `java -cp out com.ejemplo.Main`. Si hay varias fuentes, puedes compilar todas: `javac -d out src/**/*.java`.

Java es un lenguaje de **compilación a bytecode** y **ejecución sobre máquina virtual**. No se compila directamente a código nativo del SO (como C/C++), sino a un formato intermedio portable. Por eso suele decirse que es “compilado e interpretado/JIT”: se compila a bytecode y luego la JVM lo interpreta y/o lo compila en caliente a nativo (JIT) para ganar rendimiento.

La **Máquina Virtual de Java (JVM)** es el entorno de ejecución que carga clases, gestiona memoria (incluida la recolección de basura), aplica seguridad y ejecuta el bytecode de forma independiente del sistema operativo. Gracias a la JVM, el mismo programa `.class` puede ejecutarse en Windows, Linux o macOS sin recompilar, siempre que exista una JVM compatible.

El **bytecode** es el conjunto de instrucciones intermedias generado por `javac`. Cada clase compilada se guarda en un fichero **`.class`** que contiene ese bytecode. En tiempo de ejecución, la JVM carga estos `.class`, verifica su validez y los ejecuta; además, el **JIT** puede traducir partes del bytecode a **código nativo** y optimizarlas dinámicamente según el perfil real de ejecución.

## 11. En el código anterior de la clase `Punto` ¿Qué es `new`? ¿Qué es un **constructor**? Pon un ejemplo de constructor en una clase `Empleado` que tenga DNI, nombre y apellidos

**`new`** es el operador que **crea una nueva instancia** de una clase en Java. Reserva memoria en el *heap*, inicializa el objeto y devuelve una referencia a él. Cuando escribes `new Punto(3, 4)`, la JVM asigna espacio para un `Punto`, llama a su **constructor** con los argumentos `3` y `4`, y te da una referencia para usarlo.

Un **constructor** es un método especial cuyo nombre coincide con el de la clase y **no tiene tipo de retorno**. Sirve para **inicializar** el estado del objeto al momento de su creación (asignar valores a atributos, validar datos, etc.). Si no defines ninguno, Java genera un **constructor por defecto** sin parámetros; si defines al menos uno, ese constructor por defecto deja de existir.

Ejemplo sencillo con una clase `Empleado` que tiene `dni`, `nombre` y `apellidos`, con un constructor que inicializa los tres campos:

class Empleado {
    String dni;       // visibilidad por defecto (package-private)
    String nombre;    // visibilidad por defecto
    String apellidos; // visibilidad por defecto

    // Constructor
    Empleado(String dni, String nombre, String apellidos) {
        this.dni = dni;
        this.nombre = nombre;
        this.apellidos = apellidos;
    }

    // Ejemplo de uso
    public static void main(String[] args) {
        Empleado e = new Empleado("12345678Z", "Ana", "Pérez Gómez");
        System.out.println(e.nombre + " " + e.apellidos + " (" + e.dni + ")");
    }
}

## 12. ¿Qué es la referencia `this`? ¿Se llama igual en todos los lenguajes? Pon un ejemplo del uso de `this` en la clase `Punto`

La referencia **`this`** es un puntero implícito al **objeto actual**: dentro de una instancia, `this` permite acceder a sus atributos y métodos, y se usa para **desambiguar** cuando los nombres de parámetros coinciden con los campos (por ejemplo, en constructores o *setters*). También sirve para pasar la referencia del objeto actual a otros métodos o para **encadenar llamadas** (retornando `this`).

No se llama igual en todos los lenguajes. En **Java, C# y C++** se usa `this`; en **JavaScript** también es `this` pero su valor puede variar según el contexto de invocación; en **Python** se usa el parámetro convencional **`self`**; en **Swift** y **Kotlin** suele usarse **`self`** y **`this`** respectivamente. La idea es la misma: referirse a la instancia que está ejecutando el método.

Ejemplo en `Punto` usando `this` para desambiguar y encadenar:


class Punto {
    double x;
    double y;

    // Constructor: desambiguación con `this`
    Punto(double x, double y) {
        this.x = x;   // `this.x` es el atributo; `x` es el parámetro
        this.y = y;
    }

    // Setter fluido que retorna la propia instancia
    Punto mover(double dx, double dy) {
        this.x += dx;
        this.y += dy;
        return this; // permite encadenar: p.mover(1,2).mover(-1,0);
    }

    double calculaDistanciaAOrigen() {
        return Math.sqrt(this.x * this.x + this.y * this.y);
    }
}

## 13. Añade ahora otro nuevo método que se llame `distanciaA`, que reciba un `Punto` como parámetro y calcule la distancia entre `this` y el punto proporcionado

class Punto {
    double x;
    double y;

    Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    double calculaDistanciaAOrigen() {
        return Math.sqrt(this.x * this.x + this.y * this.y);
    }

    // Nuevo método: distancia entre this y otro punto
    double distanciaA(Punto otro) {
        double dx = this.x - otro.x;
        double dy = this.y - otro.y;
        return Math.sqrt(dx * dx + dy * dy);
    }

    public static void main(String[] args) {
        Punto p1 = new Punto(3, 4);
        Punto p2 = new Punto(0, 0);
        System.out.println("Distancia p1 a origen: " + p1.calculaDistanciaAOrigen()); // 5.0
        System.out.println("Distancia p1 a p2: " + p1.distanciaA(p2));                // 5.0
    }
}

## 14. El paso del `Punto` como parámetro a un método, es **por copia** o **por referencia**, es decir, si se cambia el valor de algún atributo del punto pasado como parámetro, dichos cambios afectan al objeto fuera del método? ¿Qué ocurre si en vez de un `Punto`, se recibiese un entero (`int`) y dicho entero se modificase dentro de la función? 

En **Java todo es paso por valor**. Cuando pasas un objeto como parámetro, lo que se copia es **el valor de la referencia** (una “flecha” que apunta al objeto en el heap). Por eso, si dentro del método haces `p.x = 10;`, **sí** se ve reflejado fuera, porque tanto el parámetro como la variable del llamador apuntan al **mismo objeto**. En cambio, si haces `p = new Punto(...)` dentro del método, solo cambias **la copia local de la referencia**: fuera del método no cambia qué objeto se está usando.


void modifica(Punto p) { p.x = 99; }     // afecta al objeto externo
void reasigna(Punto p) { p = new Punto(0,0); } // NO afecta fuera

Punto a = new Punto(1,1);
modifica(a);  // a.x pasa a 99
reasigna(a);  // 'a' sigue apuntando al mismo objeto (x=99, y=1)


Con tipos primitivos como `int`, lo que se copia es **el valor** en sí. Cualquier cambio dentro del método **no altera** la variable del llamador:


void inc(int n) { n++; }
int x = 5;
inc(x);   // x sigue siendo 5


En resumen: objetos → se copia la **referencia** (modificar campos afecta fuera, reasignar la referencia no); primitivos → se copia el **valor** (nada cambia fuera).


## 15. ¿Qué es el método `toString()` en Java? ¿Existe en otros lenguajes? Pon un ejemplo de `toString()` en la clase `Punto` en Java

`toString()` es el método que devuelve una **representación textual** de un objeto en Java. Todas las clases lo heredan de `java.lang.Object`, y se usa automáticamente en contextos como `System.out.println(obj)` o concatenación con cadenas. Sobrescribirlo (`@Override`) es buena práctica para obtener salidas legibles y útiles para depuración o logging (en lugar del formato por defecto `Clase@hashcode`).

Existen equivalentes en otros lenguajes: en **C#** también se llama `ToString()`, en **Python** su análogo es `__str__` (y `__repr__` para una representación más técnica), en **JavaScript** existe `toString()`, y en **C++** se suele lograr con una sobrecarga del operador `<<` para `std::ostream`. La idea común es proporcionar una **descripción humana** del estado del objeto.

**Ejemplo en `Punto`:**

class Punto {
    double x;
    double y;

    Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    @Override
    public String toString() {
        return "Punto(" + x + ", " + y + ")";
    }

    public static void main(String[] args) {
        Punto p = new Punto(3, 4);
        System.out.println(p);           // imprime: Punto(3.0, 4.0)
        System.out.println(p.toString()); // equivalente
    }
}

## 16. Reflexiona: ¿una clase es como un `struct` en C? ¿Qué le falta al `struct` para ser como una clase y las variables de ese tipo ser instancias?

En **C (clásico)**, un `struct` es solo un **agregado de datos**: no tiene métodos, ni encapsulación (modificadores de acceso), ni constructores/destructores, ni herencia, ni polimorfismo. Una **clase**, en cambio, combina **datos + comportamiento**, permite **ocultar información** (private/protected/public), definir **invariantes** mediante constructores, y participar en **jerarquías** (herencia) con **despacho dinámico** (polimorfismo).

¿Qué le “falta” al `struct` de C para ser como una clase? Principalmente: **métodos miembros**, **encapsulación real**, **constructores/destructores**, **herencia** y **métodos virtuales** (o su equivalente). En C puedes aproximarlo con funciones externas que reciben punteros al `struct`, inicializadores manuales, e incluso **punteros a función** dentro del `struct` para simular métodos, pero es solo un patrón, no soporte del lenguaje. Aun así, las **variables de un `struct` sí son instancias** de ese tipo, pero carecen de las capacidades de orientación a objetos propias de una clase.

Nota: en **C++**, `struct` y `class` son casi equivalentes; la diferencia principal es el **acceso por defecto** (`struct` → `public`, `class` → `private`) y la herencia por defecto. Ambos pueden tener métodos, constructores, herencia y polimorfismo. En **C**, no.

## 17. Quitemos un poco de magia a todo esto: ¿Como se podría “emular”, con `struct` en C, la clase `Punto`, con su función para calcular la distancia al origen? ¿Qué ha pasado con `this`?

En **C** no hay clases ni métodos, pero puedes “emular” una clase `Punto` con un `struct` para los **datos** y funciones libres para el **comportamiento**. Una función “constructor” puede inicializar el `struct`, y otra función calcular la distancia al origen usando `sqrt` de `<math.h>`. Es un patrón habitual: `Tipo_función(&instancia, …)` o `función(Tipo* self, …)`.

#include <stdio.h>
#include <math.h>

typedef struct {
    double x;
    double y;
} Punto;

Punto punto_new(double x, double y) {  // “constructor” simple
    Punto p = { x, y };
    return p;
}

double punto_distancia_a_origen(const Punto* self) {
    return sqrt(self->x * self->x + self->y * self->y);
}

int main(void) {
    Punto p = punto_new(3, 4);
    printf("Distancia al origen: %.2f\n", punto_distancia_a_origen(&p)); // 5.00
    return 0;
}

¿Qué pasó con `this`? En C **no existe `this`**: lo **simulamos** pasando explícitamente un puntero a la instancia (convención `self` o `this`) como primer parámetro de las funciones que actúan “como métodos”. Si quisieras acercarte más a la sintaxis OO, podrías incluir **punteros a función** dentro del `struct`, pero sigue siendo azúcar sintáctico; el lenguaje no aporta encapsulación, herencia ni despacho dinámico de forma nativa.
