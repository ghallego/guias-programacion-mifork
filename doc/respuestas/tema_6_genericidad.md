<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Genericidad". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: clases y objetos, encapsulación, excepciones, composición, herencia y polimorfismo.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 6. Genericidad

## 1. Empleando `void*` en C o `Object` en Java, pon un ejemplo de una estructura de datos, que empleando un array primitivo, permita alojar cualquier tipo de dato.

En C, utilizando void*, se puede crear una estructura de datos como un array que almacene punteros a cualquier tipo de dato. Esto permite flexibilidad, pero requiere manejo manual de memoria y conversión de tipos.

En Java, empleando Object, se puede definir una clase que use un array de Object para almacenar elementos de cualquier tipo. El ejemplo muestra una lista simple que añade y recupera elementos sin restricciones de tipo.

```java
public class ListaSimple {
    private Object[] elementos;
    private int size;

    public ListaSimple(int capacidad) {
        elementos = new Object[capacidad];
        size = 0;
    }

    public void add(Object elemento) {
        if (size < elementos.length) {
            elementos[size++] = elemento;
        }
    }

    public Object get(int index) {
        return elementos[index];
    }
}
```

## 2. Brevemente, ¿Qué significa la **programación genérica**? ¿Es el ejemplo anterior un ejemplo básico de programación genérica?

La programación genérica permite escribir código que puede operar con diferentes tipos de datos de manera segura y reutilizable. Se basa en parámetros de tipo que se especifican en tiempo de compilación, evitando errores en tiempo de ejecución.

El ejemplo anterior es un intento básico de programación genérica, ya que permite almacenar cualquier tipo, pero carece de verificación de tipos en compilación, lo que puede llevar a errores.

## 3. Indica los problemas respecto al chequeo de tipos, de emplear `void*` o `Object` cuando se crean estructuras de datos genéricas.

Emplear void* en C o Object en Java para estructuras de datos genéricas implica problemas con el chequeo de tipos. No hay verificación en compilación, lo que permite insertar tipos incorrectos sin detección.

Esto requiere conversiones explícitas al recuperar elementos, potencialmente causando errores en tiempo de ejecución si se hace un casting incorrecto. Además, se pierde la seguridad de tipos que previene bugs comunes.


## 4. ¿Qué son los parámetros de tipo?

Los parámetros de tipo son marcadores de posición que representan tipos específicos en definiciones de clases o métodos genéricos. Permiten que el código sea parametrizado por tipos, especificados al instanciar la clase o llamar el método.

Esto mejora la reutilización del código y proporciona verificación de tipos en compilación, evitando errores comunes asociados con el uso de tipos genéricos sin restricciones.


## 5. ¿Cómo se declaran y usan los generics en Java y C++?

En Java, los generics permiten definir clases como List<String> que solo aceptan String. En C++, los templates generan código específico para cada tipo, como vector<string>.

En el ejemplo, se crea una lista de String, se añaden valores y se recorre mostrando que cada elemento es String sin necesidad de casting, garantizando seguridad de tipos.

```java
// Java
List<String> lista = new ArrayList<>();
lista.add("Hola");
lista.add("Mundo");
for (String s : lista) {
    System.out.println(s.length()); // Seguro, s es String
}
```

```cpp
// C++
#include <vector>
#include <string>
#include <iostream>
std::vector<std::string> vec;
vec.push_back("Hola");
vec.push_back("Mundo");
for (const std::string& s : vec) {
    std::cout << s.length() << std::endl; // Seguro, s es string
}
```


## 6. ¿Qué ocurre durante la instanciación de clases genéricas?

Cuando se instancia una clase con parámetros de tipo, el compilador verifica los tipos en tiempo de compilación. En Java, utiliza type erasure, eliminando la información de tipos genéricos en el bytecode, convirtiéndolos a Object.

En C++, la instanciación de plantillas genera código específico para cada combinación de tipos utilizada, creando versiones separadas del código. Esto difiere de Java, donde no hay generación de código adicional por tipo.


## 7. ¿Cómo se declara una clase genérica?

La clase Par genérica permite almacenar dos valores de tipos diferentes. Incluye un constructor que toma ambos valores y getters para acceder a ellos.

En el ejemplo de uso, se define un método que calcula la media y desviación típica de un array de double, devolviendo un Par<Double, Double>.

```java
public class Par<T, U> {
    private T primero;
    private U segundo;

    public Par(T primero, U segundo) {
        this.primero = primero;
        this.segundo = segundo;
    }

    public T getPrimero() {
        return primero;
    }

    public U getSegundo() {
        return segundo;
    }
}

// Ejemplo de uso
public static Par<Double, Double> calcularEstadisticas(double[] datos) {
    double suma = 0;
    for (double d : datos) suma += d;
    double media = suma / datos.length;
    double varianza = 0;
    for (double d : datos) varianza += Math.pow(d - media, 2);
    double desviacion = Math.sqrt(varianza / datos.length);
    return new Par<>(media, desviacion);
}
```


## 8. ¿Cómo se declara un método genérico?

Los métodos genéricos en Java permiten parámetros de tipo a nivel de método. El método seleccionaUno toma dos objetos del mismo tipo T y devuelve uno aleatoriamente.

Comparado con usar Object, evita downcasting ya que el tipo se conoce, y garantiza que ambos parámetros sean del mismo tipo, no permitiendo mezclar tipos diferentes.

```java
// Con Object
public static Object seleccionaUno(Object a, Object b) {
    return Math.random() < 0.5 ? a : b;
}
// Requiere casting: String s = (String) seleccionaUno("a", "b");

// Con generics
public static <T> T seleccionaUno(T a, T b) {
    return Math.random() < 0.5 ? a : b;
}
// Seguro: String s = seleccionaUno("a", "b");
```


## 9. ¿Qué son los límites (bounds)?

Sí, se pueden establecer restricciones en los parámetros de tipo usando bounds. Por ejemplo, <T extends Number> requiere que T sea Number o subclase, permitiendo tratarlo como número.

En el ejemplo, Punto usa Number para coordenadas, pero con generics se refuerza el tipo específico. Tras type erasure, el tipo final es Number.

```java
// Sin generics reforzados
public class Punto {
    private Number x, y;
    public Punto(Number x, Number y) { this.x = x; this.y = y; }
    public Number getX() { return x; }
    public Number getY() { return y; }
    public double calcularDistanciaA(Punto otro) {
        double dx = x.doubleValue() - otro.x.doubleValue();
        double dy = y.doubleValue() - otro.y.doubleValue();
        return Math.sqrt(dx*dx + dy*dy);
    }
}

// Con generics
public class Punto<T extends Number> {
    private T x, y;
    public Punto(T x, T y) { this.x = x; this.y = y; }
    public T getX() { return x; }
    public T getY() { return y; }
    public double calcularDistanciaA(Punto<T> otro) {
        double dx = x.doubleValue() - otro.x.doubleValue();
        double dy = y.doubleValue() - otro.y.doubleValue();
        return Math.sqrt(dx*dx + dy*dy);
    }
}
```


Ambas soluciones permiten trabajar con diferentes tipos de número sin duplicar código, pero los generics refuerzan el chequeo de tipos. Ninguna permite crear un punto con coordenadas de tipos diferentes, ya que ambas requieren consistencia.

En la solución sin generics, getX devuelve Number, requiriendo casting para usar métodos específicos. Con generics, getX devuelve el tipo exacto T, evitando casting innecesario.


## 10. ¿Qué es la genericidad en colecciones?

Añadiendo generics a la interfaz Punto y sus implementaciones, se asegura que distanciaA tome un Punto del mismo tipo, eliminando la necesidad de instanceof y downcasting.

La interfaz genérica Punto<T> define distanciaA(Punto<T> p), y las clases implementan con el tipo específico.

```java
public interface Punto<T extends Punto<T>> {
    double distanciaA(T p);
}

public class Punto2D implements Punto<Punto2D> {
    private final double x, y;
    public Punto2D(double x, double y) {
        this.x = x; this.y = y;
    }

    @Override
    public double distanciaA(Punto2D p) {
        return Math.sqrt(Math.pow(x - p.x, 2) + Math.pow(y - p.y, 2));
    }
}

public class Punto3D implements Punto<Punto3D> {
    private final double x, y, z;
    public Punto3D(double x, double y, double z) {
        this.x = x; this.y = y; this.z = z;
    }

    @Override
    public double distanciaA(Punto3D p) {
        return Math.sqrt(Math.pow(x - p.x, 2) + Math.pow(y - p.y, 2) + Math.pow(z - p.z, 2));
    }
}
```


## 11. ¿Cuáles son las limitaciones de los generics en Java?

List<String> no es subtipo de List<Object> porque los tipos genéricos son invariantes por defecto, para mantener seguridad de tipos. String[] sí es subtipo de Object[] debido a la covarianza de arrays en Java.

Con arrays, se puede asignar String[] a Object[], pero intentar añadir un Integer causaría ClassCastException en tiempo de ejecución. Un tipo genérico es covariante si A<T> es subtipo de A<U> cuando T es subtipo de U, contravariante si al revés, e invariante si no hay relación.


## 12. ¿Cómo se declara un método con comodines?

Los wildcards (?) permiten flexibilidad en tipos genéricos. ? extends T indica un subtipo desconocido de T, usado para lectura. ? super T indica un supertipo desconocido de T, usado para escritura.

En el ejemplo, el método suma usa ? extends Number para leer números y calcular suma. El método añadir usa ? super Integer para añadir enteros a una lista.

```java
public static double suma(List<? extends Number> lista) {
    double total = 0;
    for (Number n : lista) {
        total += n.doubleValue();
    }
    return total;
}

public static void añadirEnteros(List<? super Integer> lista) {
    lista.add(1);
    lista.add(2);
}
```
