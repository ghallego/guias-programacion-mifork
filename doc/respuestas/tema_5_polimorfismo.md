<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Polimorfismo". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación, Excepciones, Composición y Herencia.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# Tema 5. Polimorfismo

## 1. Brevemente, ¿qué es el **"polimorfismo"** y para qué sirve en programación orientada a objetos? ¿qué es la **"sobreescritura"** de métodos?
El polimorfismo en la programación orientada a objetos permite que objetos de diferentes clases sean tratados como objetos de una clase común, generalmente una superclase. Esto facilita la escritura de código más flexible y extensible, ya que el mismo método puede comportarse de manera diferente según el tipo real del objeto.

La sobreescritura de métodos ocurre cuando una subclase proporciona una implementación específica de un método definido en su superclase. Esto permite a la subclase modificar o extender el comportamiento heredado, siendo fundamental para lograr polimorfismo.


## 2. ¿En qué consiste la **"ligadura dinámica"** o **"enlace tardío"**? ¿qué relación tiene con el polimorfismo? ¿hay que indicarlos explícitamente al programar o depende esto del lenguaje? Compara C++ y Java. Indicalo después también para Python.
La ligadura dinámica o enlace tardío consiste en que la decisión de qué método ejecutar se toma en tiempo de ejecución, basándose en el tipo real del objeto, no en el tipo de la referencia. Esto se relaciona directamente con el polimorfismo, ya que permite que el método correcto sea invocado según la subclase del objeto.

En Java, la ligadura dinámica es automática para métodos de instancia y no requiere indicación explícita. En C++, se debe declarar el método como virtual en la superclase para habilitar el enlace tardío. En Python, el enlace es dinámico por defecto, similar a Java, sin necesidad de palabras clave adicionales.


## 3. Pon un ejemplo sencillo en Java, de un `Soldado`, con un método `saluda`, con dos subclases: `Zapador` y `Artillero`, donde `Zapador` sobreescribe el método `saludar`, sustituyendo por completo su comportamiento. Ilustra el funcionamiento del polimorfismo creando un array de `Soldados` de dos tipos y luego recorriéndolo empleando referencias de tipo `Soldado` y llamando a `saludar`.
En el ejemplo, la clase Soldado tiene un método saludar que imprime un saludo básico. La subclase Zapador sobreescribe completamente el método saludar para proporcionar un saludo específico. La subclase Artillero mantiene el comportamiento heredado.

Para ilustrar el polimorfismo, se crea un array de Soldado que contiene instancias de diferentes subclases. Al recorrer el array con referencias de tipo Soldado y llamar a saludar, se invoca el método correspondiente a cada tipo real de objeto.

```java
public class Soldado {
    public void saludar() {
        System.out.println("Hola, soy un soldado");
    }
}

public class Zapador extends Soldado {
    @Override
    public void saludar() {
        System.out.println("Hola, soy un zapador");
    }
}

public class Artillero extends Soldado {
    // Hereda el método saludar sin cambios
}

// En el main:
Soldado[] soldados = new Soldado[3];
soldados[0] = new Soldado();
soldados[1] = new Zapador();
soldados[2] = new Artillero();

for (Soldado s : soldados) {
    s.saludar();
}
```


## 4. Si sobreescribo un método, ¿puedo invocar el método base para trabajar a partir de su resultado? Haz que zapador cambie ligeramente la forma de saludar, que salude de forma normal, tal cual hace el soldado base, pero que además añada un "ZAPADOR A SUS ORDENES" ¿qué palabra clave del lenguaje has usado para invocar al método de la clase base?
Sí, al sobreescribir un método, se puede invocar el método de la clase base utilizando la palabra clave super. Esto permite trabajar a partir del resultado del método base, añadiendo o modificando el comportamiento.

En el ejemplo, el Zapador llama primero a super.saludar() para realizar el saludo normal del Soldado, y luego añade su mensaje específico "ZAPADOR A SUS ORDENES".

```java
public class Zapador extends Soldado {
    @Override
    public void saludar() {
        super.saludar();
        System.out.println("ZAPADOR A SUS ORDENES");
    }
}
```


## 5. Al sobreescribir un método en Java, ¿qué restricciones existen sobre los tipos de los parámetros y el tipo de retorno? ¿Qué diferencia hay entre sobreescritura (*overriding*) y sobrecarga (*overloading*)? ¿Para qué sirve la anotación `@Override` y por qué es recomendable usarla siempre?
Al sobreescribir un método en Java, los tipos de los parámetros deben ser idénticos, y el tipo de retorno puede ser el mismo o un subtipo covariante. No se permiten cambios en el nombre del método o en los parámetros para mantener la compatibilidad.

La diferencia entre sobreescritura (overriding) y sobrecarga (overloading) es que la sobreescritura ocurre en subclases con la misma signatura, mientras que la sobrecarga se da en la misma clase con el mismo nombre pero parámetros diferentes. La anotación @Override se utiliza para indicar explícitamente que se está sobreescribiendo un método, y es recomendable porque ayuda al compilador a detectar errores si no se cumple la sobreescritura correcta.


## 6. Entonces, cuando se estudia Java, ¿se emplea el polimorfismo desde el principio? Por ejemplo, sobreescribiendo `toString` o sobreescribiendo `equals`, ¿ya estoy usando polimorfismo?
En Java, el polimorfismo se emplea desde el principio, ya que todas las clases heredan de Object y pueden sobreescribir sus métodos. Al sobreescribir métodos como toString o equals, se está utilizando polimorfismo, puesto que el comportamiento varía según la clase concreta del objeto.

Esto permite que código genérico funcione con diferentes tipos de objetos, aprovechando la ligadura dinámica para invocar la implementación correcta.


## 7. ¿Qué es una **"clase abstracta"**? ¿Qué es un **"método abstracto"**? ¿Puedo crear instancias de una clase abstracta? Pongamos un ejemplo en Java: Redefinamos `Soldado`, hagamos que, además del método `saluda` que ya tenía, tenga un método `atacar`, que sea abstracto y que cada tipo de soldado haga su acción cuando se le pida atacar. ¿Donde debemos poner `abstract`?
Una clase abstracta es una clase que no puede ser instanciada directamente y sirve como base para otras clases. Un método abstracto es un método declarado sin implementación, que debe ser sobreescrito en las subclases concretas. No se pueden crear instancias de una clase abstracta.

En el ejemplo, Soldado se declara como abstracta y tiene un método abstracto atacar. Las subclases Zapador y Artillero implementan atacar con sus acciones específicas. La palabra abstract se coloca antes de class para la clase y antes del tipo de retorno para el método.

```java
public abstract class Soldado {
    public void saludar() {
        System.out.println("Hola, soy un soldado");
    }

    public abstract void atacar();
}

public class Zapador extends Soldado {
    @Override
    public void atacar() {
        System.out.println("Poniendo minas");
    }
}

public class Artillero extends Soldado {
    @Override
    public void atacar() {
        System.out.println("Disparando cohetes");
    }
}
```


## 8. ¿Qué efecto tiene la palabra clave `final` sobre métodos y clases en Java? ¿Cómo se relaciona con el polimorfismo? ¿Conoces algún ejemplo de clase `final` en la propia API estándar de Java?
La palabra clave final en Java impide modificaciones: un método final no puede ser sobreescrito, y una clase final no puede ser heredada. Esto se relaciona con el polimorfismo al restringir la posibilidad de cambiar comportamientos en subclases, asegurando que el método o clase mantenga su implementación original.

En la API estándar de Java, un ejemplo de clase final es String, que no puede ser subclaseada para mantener la inmutabilidad y seguridad.


## 9. En Java, qué son las **"interfaces"**? ¿Son como clases abstractas? ¿Una clase puede implementar más de una interfaz?
Las interfaces en Java son tipos abstractos que definen un conjunto de métodos que las clases deben implementar. Son similares a clases abstractas puras, ya que no contienen implementaciones concretas, pero permiten definir contratos de comportamiento.

Una clase puede implementar más de una interfaz, lo que proporciona mayor flexibilidad que la herencia simple de clases. Esto permite que una clase tenga múltiples roles sin las limitaciones de la herencia múltiple de clases.


## 10. Vamos a poner un ejemplo nuevo con polimorfismo. Queremos implementar una clase `Punto`, con un método `calcularDistanciaA`, que permite calcular la distancia a otro `Punto`. Sin embargo, como queremos trabajar con puntos 2D y 3D, haz que ese método sea abstracto y haya dos implementaciones de ese cálculo de distancia. Emplea `instanceof` y *downcasting* para verificar que se recibe un punto compatible y poder calcular correctamente la distancia siempre entre puntos del mismo subtipo. Aprovecha este diseño para crear ahora una clase `Linea`, que acepta `Punto`, sin saber de qué tipo es, y es capaz de dar su longitud independientemente de las dimensiones de sus puntos (las cuales desconoce).
En el ejemplo, la clase abstracta Punto tiene un método abstracto calcularDistanciaA. Las subclases Punto2D y Punto3D implementan el cálculo de distancia específico para sus dimensiones. Se utiliza instanceof y downcasting para verificar el tipo del otro punto y calcular correctamente.

La clase Linea acepta Puntos sin conocer su tipo específico y calcula la longitud llamando a calcularDistanciaA, aprovechando el polimorfismo.

```java
public abstract class Punto {
    public abstract double calcularDistanciaA(Punto otro);
}

public class Punto2D extends Punto {
    private double x, y;

    public Punto2D(double x, double y) {
        this.x = x;
        this.y = y;
    }

    @Override
    public double calcularDistanciaA(Punto otro) {
        if (otro instanceof Punto2D) {
            Punto2D p = (Punto2D) otro;
            return Math.sqrt((x - p.x) * (x - p.x) + (y - p.y) * (y - p.y));
        }
        throw new IllegalArgumentException("Tipo incompatible");
    }
}

public class Punto3D extends Punto {
    private double x, y, z;

    public Punto3D(double x, double y, double z) {
        this.x = x;
        this.y = y;
        this.z = z;
    }

    @Override
    public double calcularDistanciaA(Punto otro) {
        if (otro instanceof Punto3D) {
            Punto3D p = (Punto3D) otro;
            return Math.sqrt((x - p.x) * (x - p.x) + (y - p.y) * (y - p.y) + (z - p.z) * (z - p.z));
        }
        throw new IllegalArgumentException("Tipo incompatible");
    }
}

public class Linea {
    private Punto p1, p2;

    public Linea(Punto p1, Punto p2) {
        this.p1 = p1;
        this.p2 = p2;
    }

    public double getLongitud() {
        return p1.calcularDistanciaA(p2);
    }
}
```


## 11. ¿Qué es la **"herencia de interfaces"** en Java? ¿Existe **"herencia múltiple de interfaces"**? Pon un ejemplo de una interfaz `Fichero` que tenga un método para leer su contenido en forma de `String` y luego dicha interfaz sea extendida por otra que sea `FicheroEscribible` que permita enviar contenido e incluso eliminar el fichero.
La herencia de interfaces en Java permite que una interfaz extienda otra interfaz, heredando sus métodos. Existe herencia múltiple de interfaces, ya que una interfaz puede extender varias interfaces a la vez.

En el ejemplo, la interfaz Fichero define un método leerContenido. La interfaz FicheroEscribible extiende Fichero y añade métodos escribirContenido y eliminar.

```java
public interface Fichero {
    String leerContenido();
}

public interface FicheroEscribible extends Fichero {
    void escribirContenido(String contenido);
    void eliminar();
}
```
