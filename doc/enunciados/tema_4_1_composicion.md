<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Composición". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación y Excepciones.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# Tema 4.1. Composición


## 1. En C, podemos crear estructuras mayores **componiendo** unas con otras, que suelen describirse como "A tiene-un/tiene-varios B". Pon un ejemplo, empleando `struct`, de una línea de puntos, donde puntos tienen dos coordenadas (`x` e `y`), y la línea esta hecha de dos puntos. Incluye una función para calcular la distancia entre puntos y otra para hallar la longitud de una línea.

#include <stdio.h>
#include <math.h>

typedef struct {
    double x;
    double y;
} Punto;

typedef struct {
    Punto a;
    Punto b;
} Linea;

double distancia(Punto p1, Punto p2) {
    double dx = p2.x - p1.x;
    double dy = p2.y - p1.y;
    return sqrt(dx*dx + dy*dy);
}

double longitudLinea(Linea l) {
    return distancia(l.a, l.b);
}

int main() {
    Punto p1 = {0.0, 0.0};
    Punto p2 = {3.0, 4.0};
    Linea linea = {p1, p2};

    printf("Longitud de la línea: %.2f\n", longitudLinea(linea));
    return 0;
}

En este ejemplo se observa cómo la composición permite construir estructuras complejas a partir de otras más simples. El struct Punto representa un elemento básico con dos coordenadas, mientras que el struct Linea integra dos puntos para describir un segmento. Esta relación expresa claramente el modelo conceptual “una línea tiene dos puntos”, lo cual refleja el uso típico de la composición al estructurar datos en C.

El cálculo de la distancia entre puntos se basa en la fórmula euclidiana, lo que facilita la reutilización del código al emplearlo también para determinar la longitud de la línea. De esta forma, el programa organiza la lógica en funciones independientes, lo que mejora la claridad y favorece el mantenimiento del código. Además, resulta sencillo extender la estructura a líneas más complejas o integrar nuevos tipos de cálculos si fuese necesario.

Este tipo de composición es muy común en lenguajes sin orientación a objetos, como C, donde la relación entre entidades se define mediante la inclusión de unas estructuras dentro de otras. Mediante este enfoque se consigue expresar relaciones jerárquicas o de pertenencia sin necesidad de mecanismos más avanzados como clases o herencia, apoyándose únicamente en las capacidades básicas del lenguaje.


## 2. Ahora transforma ese ejemplo a orientación a objetos con Java, para tener un primer ejemplo de **composición** en orientación a objetos. Crea una clase `Punto`, y una clase `Linea`. La clase `Punto` debe tener un método para calcular distancia a otro `Punto` y `Linea` debe tener un método para calcular su longitud. Gracias a la ocultación de información, supera a C, garantizando que los puntos sean inmutables, al igual que la línea, que una vez creada, no queremos que se modifique de qué a qué puntos va dicha línea.  

public final class Punto {
    private final double x;
    private final double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double distanciaA(Punto otro) {
        double dx = otro.x - this.x;
        double dy = otro.y - this.y;
        return Math.sqrt(dx * dx + dy * dy);
    }

    public double getX() { return x; }
    public double getY() { return y; }
}

public final class Linea {
    private final Punto a;
    private final Punto b;

    public Linea(Punto a, Punto b) {
        this.a = a;
        this.b = b;
    }

    public double longitud() {
        return a.distanciaA(b);
    }

    public Punto getA() { return a; }
    public Punto getB() { return b; }
}

En este diseño se observa cómo la composición en orientación a objetos permite expresar relaciones del tipo “una línea tiene dos puntos” de manera natural. Al ser Punto y Linea clases independientes, cada una encapsula su propio estado y comportamiento, lo que facilita organizar el código en componentes coherentes. La inmutabilidad se garantiza declarando los atributos como final y evitando setters, de modo que una vez creado un objeto, su estado interno no pueda cambiarse desde fuera.

Este enfoque mejora respecto a la solución en C porque la información queda completamente oculta dentro de las clases, minimizando errores y evitando estados inconsistentes. Al no poder modificarse los puntos ni la línea después de su creación, se eliminan efectos secundarios indeseados y se favorece la programación más segura. Además, esta estructura es fácilmente ampliable: podrían añadirse nuevos métodos sin comprometer la integridad del diseño ni afectar al comportamiento ya establecido.

## 3. ¿Qué significa la **multiplicidad** en la composición? En el ejemplo anterior, ¿cuál es la multiplicidad entre `Linea` y `Punto`? Indícalo expresando la multiplicidad en ambas direcciones, de `Linea` a `Punto` y de `Punto` a `Linea`.

La **multiplicidad** en composición indica cuántas instancias de una clase participan en la relación respecto a otra. Se utiliza para describir cuántos objetos de un tipo *posee*, *incluye* o *contiene* un objeto de otro tipo. En el contexto de la composición, esta multiplicidad suele representar una relación fuerte de pertenencia, en la que la vida del objeto contenido depende del objeto contenedor. Por tanto, la multiplicidad expresa con precisión el número de elementos que forman parte del todo y refuerza la idea de que la composición modela una relación de “todo–parte” inseparable.

Aplicado al ejemplo anterior, una `Linea` está formada exactamente por **dos** objetos `Punto`. Esto implica que, desde la perspectiva de `Linea` hacia `Punto`, la multiplicidad es **2**. De forma más formal, se expresa como:  
**Linea → Punto: 2**  
Esto significa que toda línea debe tener dos puntos y no puede tener ni menos ni más.

En sentido contrario, un `Punto` no pertenece necesariamente a ninguna `Linea`, ya que un punto puede existir de manera independiente. Sin embargo, si participa en una línea concreta, pertenece exactamente a una. Por ello, desde `Punto` hacia `Linea`, la multiplicidad se expresa como **0..1**, indicando que un punto puede no estar asociado a ninguna línea o estar asociado a una sola. Así se expresa como:  
**Punto → Linea: 0..1**

Este esquema refleja de forma clara la composición en el ejemplo: la línea depende de sus dos puntos y no puede existir sin ellos, mientras que los puntos pueden existir por sí solos sin necesidad de que haya una línea que los utilice.


## 4. ¿Qué significa composición **fuerte** y composición **débil**? ¿Qué consecuencia implica en relación al ciclo de vida de los objetos? Indica a cuál solemos referirnos como **"asociación o agregación"** y a cuál como **"composición"** propiamente.

La **composición fuerte** describe una relación en la que el objeto “todo” es dueño exclusivo de los objetos “parte”. Esto implica que las partes **no pueden existir sin** el todo: su ciclo de vida depende completamente del objeto contenedor. Cuando el objeto principal se destruye, sus partes también deben desaparecer. Este tipo de relación expresa una unión estructural muy estrecha, donde los objetos involucrados forman una entidad indivisible dentro del modelo.

La **composición débil**, por el contrario, establece una relación en la que el “todo” utiliza o agrupa objetos, pero **no es responsable de su existencia**. En este caso, los objetos “parte” pueden existir independientemente y pueden estar asociados a varios objetos “todo” o cambiar de asociación sin dejar de existir. Por tanto, su ciclo de vida es autónomo: se crean, gestionan y destruyen de forma independiente al objeto que los agrupa.

En terminología habitual, la composición débil se conoce como **asociación** o **agregación**, ya que describe una relación de pertenencia flexible o compartida. La composición fuerte es la que se denomina **composición** propiamente dicha, pues representa la unión más poderosa y restrictiva entre objetos, donde los componentes forman parte integral del objeto principal. Esta distinción resulta fundamental al modelar sistemas orientados a objetos, ya que permite elegir el grado adecuado de dependencia y cohesión entre las clases.


## 5. Cuando una clase usa a otra al recibirla o devolverla como parámetro en algún método, al hacer `new` dentro de un método, o al usarlas como variables locales, ¿hablamos de composición o de **"dependencia"**?

Cuando una clase utiliza a otra únicamente **recibiéndola como parámetro**, **devolviéndola**, **creándola dentro de un método** o **empleándola como variable local**, no se habla de composición, sino de **dependencia**. Este tipo de relación indica simplemente que una clase necesita a otra para realizar una operación concreta, pero no implica ninguna forma de pertenencia ni vínculo estructural entre sus objetos. La duración de los objetos utilizados de esta manera depende normalmente del ámbito del método o de su creador externo, no de la clase que los usa puntualmente.

La dependencia se caracteriza por ser una relación **temporal** y **débil**: la clase depende de otra solo mientras ejecuta cierto comportamiento o para completar una tarea específica. Una vez finalizada la operación, la relación deja de existir, y no hay control sobre el ciclo de vida del objeto dependido. En este contexto, el término “usa a” describe fielmente la relación, ya que la clase no posee ni está compuesta por los objetos que emplea.

Por el contrario, hablar de **composición** implicaría que la clase “todo” contiene internamente a los objetos “parte” como atributos y controla su existencia, lo cual no ocurre en los casos descritos. Así, recibir un objeto como parámetro, devolverlo o crearlo dentro de un método expresan relaciones de dependencia operativa, no relaciones estructurales. Este matiz resulta fundamental para distinguir entre el diseño de objetos que colaboran puntualmente y aquellos que forman entidades compuestas de manera permanente.


## 6. En el ejemplo anterior de línea y punto, programa la relación entre `Linea` y `Punto` de dos formas. Una **como composición fuerte**, donde el ciclo de vida de los puntos está ligado al de Linea y otra **como composición débil**, donde no.

A continuación se muestran **dos implementaciones distintas** de la relación entre `Linea` y `Punto`: una como **composición fuerte**, donde los puntos pertenecen exclusivamente a la línea y su ciclo de vida está ligado a ella, y otra como **composición débil**, donde los puntos existen independientemente y pueden ser compartidos. Cada ejemplo va acompañado de explicaciones en 2–4 párrafos, siguiendo tus requisitos.

***

# ✔️ **1. Composición fuerte (los puntos pertenecen a la línea)**

En este caso, los puntos se **crean dentro** de la clase `Linea`. No pueden existir fuera de ella ni ser compartidos. La línea controla completamente su ciclo de vida y, por tanto, los puntos desaparecen cuando la línea deja de existir. Esta es la composición “fuerte” o composición en sentido estricto.

### Código

```java
public final class Linea {
    private final Punto a;
    private final Punto b;

    public Linea(double ax, double ay, double bx, double by) {
        this.a = new Punto(ax, ay);   // Los puntos se crean dentro
        this.b = new Punto(bx, by);
    }

    public double longitud() {
        return a.distanciaA(b);
    }

    public Punto getA() { return a; }
    public Punto getB() { return b; }
}

public final class Punto {
    private final double x;
    private final double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double distanciaA(Punto otro) {
        double dx = otro.x - this.x;
        double dy = otro.y - this.y;
        return Math.sqrt(dx * dx + dy * dy);
    }
}
```

Aquí la composición fuerte queda reflejada en que `Linea` recibe valores primitivos y crea internamente los puntos. Con ello, la línea **es dueña** de los puntos y nadie más puede acceder a ellos salvo a través de la propia línea. Esta estrategia garantiza encapsulación estricta, evita referencias compartidas y refleja la relación “parte–todo” en su forma más rígida.

El ciclo de vida está claramente ligado: si la línea se destruye o deja de existir, también lo hacen los puntos. No hay forma de reutilizar esos puntos en otros objetos, ya que no son accesibles desde fuera ni fueron creados externamente. Esto representa de la manera más fiel el concepto de composición fuerte en orientación a objetos.

***

# ✔️ **2. Composición débil (agregación: la línea usa puntos externos)**

En este diseño, los puntos **se crean fuera** de la línea y pueden ser compartidos por varios objetos. La línea se limita a referenciarlos, pero no controla su existencia. Los puntos pueden cambiar de dueño, estar en varias figuras o seguir existiendo cuando la línea desaparezca. Esto corresponde a una composición “débil”, llamada también **agregación**.

### Código

```java
public final class Linea {
    private final Punto a;
    private final Punto b;

    public Linea(Punto a, Punto b) {
        this.a = a;   // Se reciben puntos ya existentes
        this.b = b;   // No se crean aquí
    }

    public double longitud() {
        return a.distanciaA(b);
    }

    public Punto getA() { return a; }
    public Punto getB() { return b; }
}

public final class Punto {
    private final double x;
    private final double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double distanciaA(Punto otro) {
        double dx = otro.x - this.x;
        double dy = otro.y - this.y;
        return Math.sqrt(dx * dx + dy * dy);
    }
}
```

En esta versión se observa cómo la línea no es responsable del ciclo de vida de los puntos, sino que únicamente los utiliza. Al recibirlos desde el exterior, los mismos puntos podrían emplearse en múltiples líneas o en otras estructuras geométricas. Esto elimina la relación fuerte de pertenencia y establece una conexión más flexible y reutilizable entre objetos.

La composición débil refleja un tipo de relación muy frecuente en modelado: la línea “tiene” puntos, pero no son suyos; simplemente los usa. Cuando la línea desaparece, los puntos continúan existiendo sin problema. Esto marca la diferencia esencial con la composición fuerte, donde los puntos dependen por completo del objeto que los contiene.

***

Si quieres, puedo hacer también **diagramas UML**, comparaciones entre enfoques o ayudarte a aplicar estas ideas a otros ejemplos habituales (rectángulos, polígonos, etc.).


## 7. En Java, en la composición fuerte, ¿cuando el contenedor destruye los objetos? No se observa que `Linea` destruya los `Punto` explícitamente, ¿Por qué?

En Java, la destrucción de objetos no se realiza de manera explícita como en lenguajes sin gestión automática de memoria. El entorno de ejecución de Java, conocido como máquina virtual Java (JVM), incorpora un recolector de basura que se encarga de liberar la memoria ocupada por objetos que ya no son accesibles. En el caso de la composición fuerte, cuando el objeto contenedor, como una instancia de `Linea`, deja de ser referenciado por ninguna parte del programa, el recolector de basura puede reclamar tanto la memoria de la línea como la de los puntos que contiene, ya que estos puntos solo existen dentro del contexto de esa línea.

Esta ausencia de destrucción explícita se debe a que Java prioriza la seguridad y la simplicidad en la gestión de memoria, evitando errores comunes como fugas de memoria o accesos a memoria liberada. Al declarar los atributos como `final` y privados, se garantiza que los puntos no puedan ser modificados ni referenciados desde fuera, lo que facilita que el recolector de basura determine cuándo estos objetos pueden ser eliminados de forma segura. Este mecanismo automático contrasta con enfoques manuales en lenguajes como C, donde el programador debe gestionar explícitamente la liberación de memoria.

En consecuencia, la composición fuerte en Java se basa en la confianza en el recolector de basura para manejar el ciclo de vida de los objetos componentes. No es necesario escribir código para destruir los puntos, ya que el sistema se ocupa de ello cuando el contenedor ya no es necesario, lo que permite centrarse en la lógica del programa sin preocuparse por detalles de bajo nivel de gestión de memoria.


## 8. Pon un ejemplo de composicion débil entre un departamento que tiene varios profesores. Implementa dos composiciones a la vez: entre el departamento y todos sus profesores y entre el departamento y su director, que es un profesor del departamento. Siempre debe haber un director en el departamento desde el inicio. Lanza excepciones si se viola la invariante. Emplea arrays primitivos de Java, estilo `Profesor[]`, con máximo 50, pero no rompas la encapsulación, no desveles que estás empleando un array, permite añadir un `Profesor` al final de la lista, y eliminar un profesor dada su posición. Da acceso a los profesores con un método para saber cuántos hay y otro para obtener un profesor por posición. El director se puede cambiar por otro profesor del departamento. Sin embargo, ten en cuenta esta invariante de clase: el director debe formar siempre parte de la lista de profesores, es decir, ten cuidado al cambiar el director o al eliminar un profesor.

En este ejemplo se implementa una composición débil entre `Departamento` y `Profesor`, donde el departamento agrupa profesores existentes sin controlar su ciclo de vida. Se incluyen dos relaciones: una con todos los profesores y otra específica con el director, que debe ser uno de los profesores. La invariante se mantiene lanzando excepciones cuando se intenta violarla, como eliminar el director o asignar uno que no esté en la lista.

La clase `Profesor` se mantiene simple, encapsulando únicamente el nombre. El `Departamento` utiliza un array interno de tamaño fijo para almacenar hasta 50 profesores, pero oculta esta implementación mediante métodos que gestionan la adición y eliminación sin exponer el array. El constructor asegura que siempre haya un director inicial, añadiéndolo automáticamente a la lista.

```java
public class Profesor {
    private final String nombre;

    public Profesor(String nombre) {
        this.nombre = nombre;
    }

    public String getNombre() {
        return nombre;
    }
}

public class Departamento {
    private final Profesor[] profesores = new Profesor[50];
    private int numProfesores = 0;
    private Profesor director;

    public Departamento(Profesor directorInicial) {
        setDirector(directorInicial);
        addProfesor(directorInicial);
    }

    public void addProfesor(Profesor p) {
        if (numProfesores >= 50) {
            throw new IllegalStateException("No se pueden añadir más de 50 profesores");
        }
        profesores[numProfesores++] = p;
    }

    public void removeProfesor(int pos) {
        if (pos < 0 || pos >= numProfesores) {
            throw new IndexOutOfBoundsException("Posición inválida");
        }
        if (profesores[pos] == director) {
            throw new IllegalArgumentException("No se puede eliminar al director");
        }
        for (int i = pos; i < numProfesores - 1; i++) {
            profesores[i] = profesores[i + 1];
        }
        numProfesores--;
    }

    public void setDirector(Profesor p) {
        boolean encontrado = false;
        for (int i = 0; i < numProfesores; i++) {
            if (profesores[i] == p) {
                encontrado = true;
                break;
            }
        }
        if (!encontrado) {
            throw new IllegalArgumentException("El director debe estar en la lista de profesores");
        }
        director = p;
    }

    public int getNumProfesores() {
        return numProfesores;
    }

    public Profesor getProfesor(int pos) {
        if (pos < 0 || pos >= numProfesores) {
            throw new IndexOutOfBoundsException("Posición inválida");
        }
        return profesores[pos];
    }

    public Profesor getDirector() {
        return director;
    }
}
```

Este diseño mantiene la encapsulación al no devolver el array interno, sino copias o elementos individuales. La composición débil permite que los profesores existan independientemente y puedan ser compartidos, mientras que las excepciones protegen la invariante de que el director siempre forme parte del departamento.


## 9. En Java, existen también `List`, cambia y muestra cómo sería el código anterior empleando `List` en vez de arrays primitivos. ¿Qué parte del código original te has ahorrado? Además, fíjate en el método `getProfesor(int pos)`: si en su lugar existiera un método que devolviera todos los profesores a la vez, ¿qué problema tendría devolver directamente la lista interna? ¿Cómo lo resolverías?

Al cambiar de arrays primitivos a `List`, se simplifica considerablemente la gestión de la colección de profesores. La interfaz `List` proporciona métodos integrados para añadir, eliminar y acceder a elementos, eliminando la necesidad de implementar manualmente el desplazamiento de elementos al eliminar o de mantener un contador separado para el número de profesores. Esto ahorra el código para el bucle de desplazamiento en `removeProfesor` y la verificación manual de límites, ya que `List` lanza excepciones automáticamente para índices inválidos.

En el código modificado, se utiliza `ArrayList` para almacenar los profesores, manteniendo el límite máximo de 50 mediante una verificación antes de añadir. Los métodos `add` y `remove` de `List` manejan automáticamente el redimensionamiento interno, lo que reduce la complejidad del código original. Sin embargo, la lógica para verificar la invariante del director permanece similar, ya que requiere buscar en la lista si el profesor candidato está presente.

Si se implementara un método que devolviera directamente la lista interna de profesores, se rompería la encapsulación porque el código externo podría modificar la lista, añadiendo o eliminando elementos sin pasar por los métodos de control del `Departamento`. Esto violaría la invariante y permitiría cambios no autorizados. Para resolverlo, se debería devolver una vista inmodificable de la lista utilizando `Collections.unmodifiableList`, lo que permite el acceso de lectura pero previene modificaciones externas.

```java
import java.util.ArrayList;
import java.util.List;
import java.util.Collections;

public class Profesor {
    private final String nombre;

    public Profesor(String nombre) {
        this.nombre = nombre;
    }

    public String getNombre() {
        return nombre;
    }
}

public class Departamento {
    private final List<Profesor> profesores = new ArrayList<>();
    private Profesor director;

    public Departamento(Profesor directorInicial) {
        setDirector(directorInicial);
        addProfesor(directorInicial);
    }

    public void addProfesor(Profesor p) {
        if (profesores.size() >= 50) {
            throw new IllegalStateException("No se pueden añadir más de 50 profesores");
        }
        profesores.add(p);
    }

    public void removeProfesor(int pos) {
        if (pos < 0 || pos >= profesores.size()) {
            throw new IndexOutOfBoundsException("Posición inválida");
        }
        if (profesores.get(pos) == director) {
            throw new IllegalArgumentException("No se puede eliminar al director");
        }
        profesores.remove(pos);
    }

    public void setDirector(Profesor p) {
        if (!profesores.contains(p)) {
            throw new IllegalArgumentException("El director debe estar en la lista de profesores");
        }
        director = p;
    }

    public int getNumProfesores() {
        return profesores.size();
    }

    public Profesor getProfesor(int pos) {
        return profesores.get(pos);  // List lanza IndexOutOfBoundsException automáticamente
    }

    public List<Profesor> getProfesores() {
        return Collections.unmodifiableList(profesores);
    }

    public Profesor getDirector() {
        return director;
    }
}
```


## 10. Al igual que ocurre con las excepciones en Java, que pueden encerrar causas (que son excepciones), de forma recursiva, suponen un tipo especial de composiciones, denominadas composiciones recursivas. Pon un ejemplo en Java de una `Persona`, que sea inmutable, y que tiene una madre, que es otra `Persona`. Haz un main con un ejemplo de uso con una familia de personas, desde el nieto hasta la abuela. Enumera algún otro ejemplo clásico de composiciones recursivas.

Las composiciones recursivas ocurren cuando una clase se compone de instancias de sí misma, creando estructuras jerárquicas o anidadas. En el ejemplo de `Persona`, cada persona tiene una madre que es otra `Persona`, permitiendo representar árboles genealógicos. La inmutabilidad se asegura declarando los atributos como `final`, lo que impide modificaciones después de la creación y mantiene la integridad de la estructura recursiva.

El método `main` demuestra el uso creando una cadena de personas desde el nieto hasta la abuela, accediendo recursivamente a las madres. Esto ilustra cómo la composición recursiva permite modelar relaciones familiares de manera natural, donde cada persona forma parte de una jerarquía mayor.

```java
public final class Persona {
    private final String nombre;
    private final Persona madre;

    public Persona(String nombre, Persona madre) {
        this.nombre = nombre;
        this.madre = madre;
    }

    public String getNombre() {
        return nombre;
    }

    public Persona getMadre() {
        return madre;
    }
}

public class Main {
    public static void main(String[] args) {
        Persona abuela = new Persona("Abuela", null);
        Persona madre = new Persona("Madre", abuela);
        Persona nieto = new Persona("Nieto", madre);

        System.out.println("Familia:");
        Persona actual = nieto;
        while (actual != null) {
            System.out.println(actual.getNombre());
            actual = actual.getMadre();
        }
    }
}
```

Otros ejemplos clásicos de composiciones recursivas incluyen las excepciones en Java, donde una excepción puede tener una causa que es otra excepción, formando cadenas de errores; los directorios en sistemas de archivos, donde un directorio puede contener subdirectorios que son también directorios; y las estructuras de datos como árboles binarios, donde cada nodo tiene referencias a nodos hijos del mismo tipo.

## 11. ¿Qué son las relaciones de composición "bidireccionales"? ¿Qué habría que hacer para implementar este tipo de relación en el ejemplo de `Profesor` y `Departamento`?

Las relaciones de composición bidireccionales ocurren cuando ambos objetos involucrados en la composición conocen y mantienen referencias entre sí. A diferencia de las relaciones unidireccionales, donde solo uno de los objetos sabe del otro, en las bidireccionales se establece una conexión mutua que permite navegar en ambas direcciones. Esto facilita el acceso directo desde cualquier lado de la relación, pero requiere mantener la consistencia de las referencias para evitar inconsistencias.

En el ejemplo de `Profesor` y `Departamento`, la relación actual es unidireccional: el `Departamento` conoce a sus `Profesores`, pero los `Profesores` no saben a qué `Departamento` pertenecen. Para hacerla bidireccional, se debe añadir a la clase `Profesor` un atributo que referencie al `Departamento`, y actualizar esta referencia cada vez que se añade o elimina un profesor del departamento. Esto implica modificar los métodos `addProfesor` y `removeProfesor` para establecer o limpiar la referencia correspondiente.

La implementación requiere añadir un campo `departamento` en `Profesor`, con métodos para acceder y modificar esta referencia de manera controlada. Al añadir un profesor, se asigna el departamento actual; al eliminarlo, se pone a `null`. Esto asegura que la relación se mantenga sincronizada, pero aumenta la complejidad al gestionar las referencias mutuas y evitar ciclos o referencias huérfanas.
