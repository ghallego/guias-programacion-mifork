<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Herencia". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación, Excepciones y Composición.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# Tema 4.2. Herencia

## 1. En orientación a objetos, ¿qué es la **herencia** y su relación con "A es-un B"?. Explica las dos implicaciones principales: (1) **compatibilidad de tipos** y (2) **herencia de estado y comportamiento**. Pon un ejemplo en Java muy sencillo, donde un `Soldado` tiene un `nombre` (privado) y un método `saludar()` que muestra su nombre. Hay dos subtipos: un `Artillero`, que es capaz de disparar cohetes y un `Zapador` que pone minas, ambos heredan el atributo nombre y la capacidad de saludar. Además, y de forma específica, el artillero tiene un número de cohetes y el zapador un número de minas, accesibles mediante "getters" específicos. Respecto a la compatibilidad de tipos, aprovechémosla: crea un array de `Soldado`, mete varios de distinto tipo (son todos compatibles con `Soldado`). Recórrela y que todos te saluden.

La herencia en la programación orientada a objetos se define como el mecanismo mediante el cual una clase puede adquirir las propiedades y comportamientos de otra clase. Esta relación se expresa comúnmente como "A es-un B", donde A representa la subclase y B la superclase, indicando que A es un tipo específico de B. Por ejemplo, un perro es-un animal, lo que significa que la clase Perro hereda de la clase Animal.

Las implicaciones principales de la herencia son dos: la compatibilidad de tipos y la herencia de estado y comportamiento. La compatibilidad de tipos permite que objetos de subclases sean tratados como objetos de la superclase, facilitando el uso de polimorfismo en el código. La herencia de estado y comportamiento implica que las subclases adquieren automáticamente los atributos y métodos de la superclase, pudiendo añadir nuevos o modificar los existentes.

En el ejemplo proporcionado, se define una clase Soldado con un atributo privado nombre y un método público saludar que imprime el nombre. Las subclases Artillero y Zapador heredan estos elementos y añaden atributos específicos como numCohetes y numMinas, respectivamente, con sus correspondientes getters. Para demostrar la compatibilidad de tipos, se crea un array de Soldado que puede contener instancias de Artillero y Zapador, y se recorre para invocar el método saludar en cada uno.

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
    private int numCohetes;

    public Artillero(String nombre, int numCohetes) {
        super(nombre);
        this.numCohetes = numCohetes;
    }

    public int getNumCohetes() {
        return numCohetes;
    }
}

public class Zapador extends Soldado {
    private int numMinas;

    public Zapador(String nombre, int numMinas) {
        super(nombre);
        this.numMinas = numMinas;
    }

    public int getNumMinas() {
        return numMinas;
    }
}

// En el main:
Soldado[] soldados = new Soldado[3];
soldados[0] = new Soldado("General");
soldados[1] = new Artillero("Artillero1", 5);
soldados[2] = new Zapador("Zapador1", 10);

for (Soldado s : soldados) {
    s.saludar();
}
```

## 2. Al crear los soldados concretos, ¿cuántos constructores se ejecutan y en qué orden? ¿Qué significa `super` dentro de un constructor? Si la clase base no tiene visible el constructor sin parámetros, ¿debo llamar a `super` siempre?

Al crear instancias de subclases, se ejecutan constructores de la superclase y la subclase en orden jerárquico. Primero se ejecuta el constructor de la superclase, luego el de la subclase. Esto asegura que la inicialización se realice de manera ordenada, comenzando desde la clase base.

La palabra clave super dentro de un constructor se utiliza para invocar el constructor de la superclase. Permite pasar parámetros al constructor padre y garantizar que se inicialice correctamente antes de la subclase.

Si la clase base no tiene un constructor sin parámetros visible, es necesario llamar a super explícitamente en el constructor de la subclase. De lo contrario, el compilador no podrá compilar el código, ya que intenta llamar automáticamente al constructor sin parámetros de la superclase.

## 3. Respecto a los objetos de subclases en memoria, los atributos privados de la superclase, ¿forman parte de una instancia de la subclase en memoria? En caso afirmativo ¿implica que se puedan usar desde el código de la subclase? Explícalo con el ejemplo de `Soldado` y alguna de sus subclases.

Los atributos privados de la superclase forman parte de la instancia de la subclase en memoria. Sin embargo, no se pueden acceder directamente desde el código de la subclase debido a la encapsulación.

Esto significa que, aunque el atributo nombre de Soldado está presente en un objeto Artillero, el código de Artillero no puede referenciarlo directamente. Se debe utilizar métodos públicos o protegidos de la superclase para interactuar con él.

En el ejemplo, el atributo nombre es privado en Soldado, por lo que en Zapador no se puede acceder directamente a él, pero se puede usar el método saludar heredado.

## 4. ¿Qué implica en términos de **extensibilidad** de código el hecho de que sean compatibles a nivel de tipos? Ilustra esto añadiendo un nuevo tipo de `Soldado` y demostrando que el código para pedir el saludo a todos los soldados no se modifica.

La compatibilidad de tipos en herencia implica que el código es extensible, ya que se pueden añadir nuevos subtipos sin modificar el código existente que opera sobre la superclase.

Por ejemplo, al añadir un nuevo tipo de Soldado, como Infante, el código que recorre un array de Soldado y llama a saludar no necesita cambios, ya que Infante es compatible con Soldado.

Esto promueve el principio de abierto-cerrado, donde el código está abierto a extensiones pero cerrado a modificaciones.

## 5. En Java, cuando trabajo con referencias y herencia. ¿Puedo tener una referencia del supertipo que apunte a objetos reales de un subtipo? ¿Puedo invocar con la referencia del supertipo a métodos públicos del subtipo? ¿En qué consiste el **"upcasting"** y el **"downcasting"**? ¿Qué es el `instanceof`? Pon un ejemplo de recorrido de un array de `Soldado`, comprobando que, si el objeto real es un `Artillero`, solicite el número de cohetes que tiene y los imprima.

Sí, se puede tener una referencia del supertipo apuntando a objetos de un subtipo. Esto se conoce como upcasting y ocurre implícitamente.

No se pueden invocar métodos específicos del subtipo usando la referencia del supertipo, a menos que se realice downcasting. El downcasting requiere una verificación con instanceof para asegurar el tipo correcto.

El instanceof se utiliza para comprobar si un objeto es una instancia de una clase específica. En el ejemplo, al recorrer un array de Soldado, se puede usar instanceof para verificar si es un Artillero y entonces hacer downcasting para acceder a getNumCohetes.

```java
for (Soldado s : soldados) {
    s.saludar();
    if (s instanceof Artillero) {
        Artillero a = (Artillero) s;
        System.out.println("Cohetes: " + a.getNumCohetes());
    }
}
```

## 6. Respecto a la ocultación de información y herencia, ¿qué significa acceso **"protegido"** de métodos y/o atributos? ¿Cómo se implementa en Java? Pon un ejemplo de uso de en la clase `Soldado` para que su nombre sea protegido y pueda usarse en el método de poner bombas del `Zapador`.

El acceso protegido en Java se indica con la palabra clave protected, permitiendo que métodos y atributos sean accesibles dentro de la misma clase, subclases y el mismo paquete.

Esto facilita la herencia al permitir que subclases accedan a miembros internos de la superclase sin exponerlos públicamente.

En el ejemplo, cambiando nombre a protected en Soldado, el método ponerBombas en Zapador puede acceder directamente a nombre para alguna operación.

```java
public class Soldado {
    protected String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Hola, soy " + nombre);
    }
}

public class Zapador extends Soldado {
    private int numMinas;

    public Zapador(String nombre, int numMinas) {
        super(nombre);
        this.numMinas = numMinas;
    }

    public void ponerBombas() {
        System.out.println(nombre + " pone " + numMinas + " minas");
    }

    public int getNumMinas() {
        return numMinas;
    }
}
```

## 7. En los lenguajes orientados a objetos ¿hay una **clase base** para todos los objetos? ¿Ocurre en todos los lenguajes? ¿Qué ocurre en Java?

En muchos lenguajes orientados a objetos existe una clase base común para todos los objetos, conocida como Object. En Java, la clase Object es la superclase raíz de todas las clases.

Esto proporciona métodos comunes como toString, equals y hashCode a todas las instancias.

No todos los lenguajes tienen esta clase base; por ejemplo, en C++ no hay una clase base universal.

## 8. ¿Qué es la **"herencia múltiple"**? ¿Existe en Java herencia múltiple?

La herencia múltiple permite que una clase herede de más de una superclase directamente. En Java no existe herencia múltiple de clases, pero se puede simular con interfaces.

Java opta por evitar los problemas de diamante asociados con la herencia múltiple mediante el uso de interfaces para múltiples herencias.

## 9. Las excepciones en los lenguajes orientados a objetos son objetos. Por tanto, se pueden crear excepciones personalizadas. Pon un ejemplo en Java de una excepción personalizada (`UsuarioNoEncontradoException`), que sea *no controlada* y que además este compuesto con un `Usuario`, para saber qué `Usuario` dio el problema. Permite además que se pueda incluir la causa, es decir, sobrecarga el constructor para tener una versión que permita añadir la causa subyacente.

Las excepciones personalizadas en Java se crean extendiendo Exception o RuntimeException. Para una excepción no controlada, se extiende RuntimeException.

En el ejemplo, UsuarioNoEncontradoException extiende RuntimeException y está compuesta con un Usuario. Incluye constructores para mensaje y causa.

```java
public class Usuario {
    private String nombre;

    public Usuario(String nombre) {
        this.nombre = nombre;
    }

    public String getNombre() {
        return nombre;
    }
}

public class UsuarioNoEncontradoException extends RuntimeException {
    private Usuario usuario;

    public UsuarioNoEncontradoException(String message, Usuario usuario) {
        super(message);
        this.usuario = usuario;
    }

    public UsuarioNoEncontradoException(String message, Usuario usuario, Throwable cause) {
        super(message, cause);
        this.usuario = usuario;
    }

    public Usuario getUsuario() {
        return usuario;
    }
}
```

## 10. Herencia vs. Composición. Se dice que no se debe emplear herencia simplemente por reutilizar código, es decir, que si quiero reutilizar código simplemente, no debo pensar en herencia como primera opción ¿por qué?

No se debe usar herencia solo para reutilizar código porque puede llevar a jerarquías inadecuadas y acoplamiento fuerte. La composición permite reutilizar código de manera más flexible sin las restricciones de la relación "es-un".

La herencia implica una relación fuerte entre clases, mientras que la composición permite cambiar implementaciones en tiempo de ejecución.

## 11. Herencia vs. Composición. Se dice que se debe *"favorecer la composición frente a la herencia"*, ¿por qué?

Se favorece la composición sobre la herencia porque ofrece mayor flexibilidad y menor acoplamiento. Permite cambiar comportamientos dinámicamente y evita problemas de herencia múltiple.

La composición sigue el principio de favorecer objetos que tienen otros objetos sobre objetos que son otros objetos.

## 12. Herencia vs. Composición. Se dice que la *"herencia rompe la encapsulación"*, ¿a qué se refiere esto?

La herencia rompe la encapsulación porque las subclases dependen de la implementación interna de la superclase. Cambios en la superclase pueden afectar a las subclases inesperadamente.

Esto viola el principio de encapsulación al exponer detalles internos a través de la herencia.

## 13. Pongamos un ejemplo de dos alternativas para lo mismo. Tenemos un `Estudiante` y un `Trabajador`, ambos tienen datos en común: el DNI y el nombre. Modelemos esto de dos formas: uno por herencia, con una superclase `Persona`, y otro con composición, con una clase `DatosPersonales`. Se debe recibir una instancia de `DatosPersonales` en el constructor de la clase `Estudiante` y `Trabajador`.

Para modelar con herencia, se crea una superclase Persona con dni y nombre, y Estudiante y Trabajador heredan de ella.

Para composición, se crea DatosPersonales con dni y nombre, y Estudiante y Trabajador tienen una instancia de DatosPersonales pasada en el constructor.

```java
// Herencia
public class Persona {
    protected String dni;
    protected String nombre;

    public Persona(String dni, String nombre) {
        this.dni = dni;
        this.nombre = nombre;
    }
}

public class Estudiante extends Persona {
    public Estudiante(String dni, String nombre) {
        super(dni, nombre);
    }
}

public class Trabajador extends Persona {
    public Trabajador(String dni, String nombre) {
        super(dni, nombre);
    }
}

// Composición
public class DatosPersonales {
    private String dni;
    private String nombre;

    public DatosPersonales(String dni, String nombre) {
        this.dni = dni;
        this.nombre = nombre;
    }

    public String getDni() {
        return dni;
    }

    public String getNombre() {
        return nombre;
    }
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
