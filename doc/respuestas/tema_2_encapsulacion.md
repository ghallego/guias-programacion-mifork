<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Encapsulación". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 2. Encapsulación

## 1. En Programación Orientada a Objetos (POO), ¿Qué buscan la **encapsulación** y **la ocultación** de información? Enumera brevemente algunas ventajas de la ocultación de información.

La encapsulación busca agrupar datos (estado) y operaciones (comportamiento) en una misma unidad —la clase— de modo que el estado quede protegido y se acceda a él solo a través de una interfaz bien definida. La ocultación de información (information hiding) complementa esta idea: consiste en no exponer detalles internos (atributos y/o implementación) que no son necesarios para usar el objeto. De este modo, se reduce el acoplamiento entre quien usa la clase y su implementación concreta.
Entre las ventajas de ocultar información destacan: (1) robustez y mantenibilidad, ya que cambios internos no rompen a los clientes; (2) control de invariantes, al centralizar la modificación del estado en pocos puntos; (3) flexibilidad evolutiva, permitiendo mejorar el rendimiento o cambiar estructuras internas sin alterar la interfaz; y (4) reducción de errores, al prevenir modificaciones directas e incoherentes del estado.

## 2. ¿Qué se entiende por la **interfaz pública** de un objeto o clase en POO? Describe brevemente cómo se relaciona con la ocultación de información.

La interfaz pública es el conjunto de miembros accesibles desde fuera de la clase, típicamente métodos públicos (y, en contadas ocasiones, constantes públicas). Constituye el contrato de uso: qué operaciones se ofrecen, con qué parámetros, y qué efectos observables tienen (retornos, excepciones). Todo lo que no se necesita para usar la clase debería mantenerse fuera de la interfaz pública.
Su relación con la ocultación de información es directa: cuanto más precisa y mínima sea la interfaz pública, mayor libertad se tendrá para cambiar la implementación privada sin impacto en el código cliente. Esto permite evolucionar la clase y mantener sus invariantes sin comprometer compatibilidad.

## 3. Brevemente: ¿Por qué hay que ser conscientes y diseñar con cuidado la **interfaz pública** de una clase? ¿Es fácil cambiarla?

Conviene diseñar la interfaz pública con cuidado porque actúa como un contrato estable con el resto del sistema. Una interfaz mal diseñada obliga a exponer detalles innecesarios, multiplica dependencias y dificulta controlar las invariantes. Además, una buena interfaz debe ser cohesiva, mínima y consistente (nombres claros, pre/postcondiciones razonables, errores bien definidos).
Cambiar la interfaz pública no es fácil: cualquier modificación puede romper a los clientes (código que la usa). Aunque existen técnicas de evolución (sobrecargas, métodos deprecados, adaptadores), implican costes y riesgos. Por ello, se prefiere invertir en un buen diseño inicial y exponer solo lo necesario.

## 4. ¿Qué son las **invariantes de clase** y por qué la ocultación de información nos ayuda?

Las invariantes de clase son propiedades que siempre deben cumplirse para cualquier objeto válido, salvo quizá durante la ejecución interna de un método (estado transitorio). Ejemplos típicos: “el saldo nunca es negativo”, “las coordenadas están normalizadas” o “la colección no contiene nulos”.
La ocultación de información ayuda porque limita los puntos desde los que se puede mutar el estado. Si los atributos son privados y solo se modifican mediante métodos bien definidos, la clase puede verificar y preservar sus invariantes en un número reducido de lugares, lo que simplifica el razonamiento y reduce errores.

## 5. Pon un ejemplo de una clase `Punto` en `Java`, con dos coordenadas, `x` e `y`, de tipo `double`, con un método `calcularDistanciaAOrigen`, y que haga uso de la ocultación de información. ¿Cuál es la interfaz pública de la clase `Punto`? ¿Qué significa `public` y `private`?

public class Punto {
    private double x;
    private double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double getX() { return x; }

    public double getY() { return y; }

    public double calcularDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }
}

La interfaz pública de Punto son su constructor, los getters y el método calcularDistanciaAOrigen; todo lo demás (los atributos) queda oculto. En Java, public indica que el miembro es accesible desde cualquier clase; private restringe el acceso solo al código de la misma clase, protegiendo la representación interna y permitiendo mantener invariantes.

## 6. En Java, ¿A quiénes se pueden aplicar los modificadores `public` o `private`?

En Java, public puede aplicarse a clases de nivel superior, interfaces, enums y a miembros (constructores, métodos, campos) y tipos anidados. Un tipo de nivel superior no puede ser private (solo puede ser public o package‑private —sin modificador—).
private se aplica a miembros (campos, métodos, constructores) y a tipos anidados (clases/interfaces/enums dentro de otra clase). Así, se controla visibilidad fina de la implementación, manteniendo la interfaz pública limpia y el resto oculto.

## 7. En POO, la visibilidad puede ser pública o privada, pero ¿existen más tipos de visibilidad? ¿Qué ocurre en Java? ¿Y en otros lenguajes?

Además de public y private, Java ofrece protected (accesible desde la propia clase, sus subclases y el mismo paquete) y el nivel por defecto (package‑private), que habilita acceso solo dentro del mismo paquete. En total, Java tiene cuatro niveles de acceso.
En otros lenguajes hay variaciones. C++ maneja public, protected, private a nivel de clase pero no tiene noción de paquete. C# añade internal (acceso en el mismo ensamblado) y combinaciones como protected internal y private protected. Estas opciones reflejan distintas unidades de modularidad y necesidades de encapsulación.

## 8. Responde: Los miembros de instancia privados de un objeto están ocultos para (a) otras clases o (b) otras instancias, aunque sean de la misma clase. Pon un ejemplo añadiendo un método `calcularDistanciaAPunto(Punto otro)` y explica la respuesta.

Los miembros privados de instancia están ocultos para otras clases, pero no para otras instancias de la misma clase. En Java, cualquier método de Punto puede acceder a los atributos privados de otro Punto porque todo ese código pertenece a la misma clase.

public double calcularDistanciaAPunto(Punto otro) {
    double dx = this.x - otro.x;   // Acceso legal: ambos son Punto
    double dy = this.y - otro.y;
    return Math.sqrt(dx * dx + dy * dy);
}
``
Esto permite implementar operaciones entre objetos del mismo tipo sin exponer su representación. Lo que permanece prohibido es el acceso a esos campos privados desde otra clase distinta.

## 9. ¿Qué son los métodos "getter" y "setter" en los lenguajes orientados a objetos?

Los getters (accesores) son métodos que leen el valor de un atributo, y los setters (mutadores) lo modifican. Se emplean para controlar el acceso a los datos, permitiendo validar, normalizar o disparar lógica adicional (por ejemplo, actualizar métricas, mantener invariantes o lanzar excepciones ante valores inválidos).
La convención de nombres en Java es getX() / setX(...), y para booleanos suele usarse isX() como getter. Aunque son comunes, no deberían exponerse automáticamente; conviene publicar solo aquellos que tienen sentido para el contrato de la clase.

## 10. Cuando nos referimos a que la ocultación de información mejora la "seguridad" del programa, ¿nos referimos a que no pueda ser "hackeado"?

Cuando se afirma que la ocultación de información mejora la “seguridad”, se alude principalmente a la integridad del diseño: prevenir usos indebidos del estado, mantener invariantes y evitar acoplamiento. No constituye una barrera de seguridad contra ataques maliciosos al sistema (no es un sandbox ni un mecanismo criptográfico).
En Java, incluso existen vías como la reflexión (sujeta a políticas de seguridad) que pueden inspeccionar miembros privados en contextos especiales. Por tanto, la encapsulación protege el diseño y la corrección, no sustituye a medidas de seguridad (control de acceso, cifrado, validación de entradas, etc.).

## 11. ¿Qué diferencia hay entre **miembro de instancia** y **miembro de clase**? ¿Los miembros de clase también se pueden ocultar?

Un miembro de instancia pertenece a cada objeto (cada instancia tiene su propia copia). Un miembro de clase (declarado con static) pertenece a la clase en sí, compartiéndose entre todas las instancias. Se accede a los miembros de clase típicamente mediante NombreDeLaClase.miembro.
Los miembros de clase también pueden ocultarse con private o exponerse con public. La encapsulación sigue aplicando: se puede mantener estado o utilidades internas estáticas como detalles de implementación, ofreciendo una interfaz pública mínima y estable.

## 12. Brevemente: ¿Tiene sentido que los constructores sean privados?

Tiene sentido que un constructor sea privado cuando no se desea permitir la creación directa de instancias desde fuera. Es útil en clases de utilidades (para impedir instanciación), patrones Singleton o cuando se fuerza la creación mediante métodos factoría estáticos que pueden validar, cachear o escoger subtipos.
También se emplea con clases inmutables y con builders, donde se oculta el constructor y se guía al usuario por un proceso de construcción controlado. De este modo, se asegura que toda instancia nace en un estado válido y conforme a las invariantes.

## 13. ¿Cómo se indican los **miembros de clase** en Java? Pon un ejemplo, en la clase `Punto` definida anteriormente, para que incluya miembros de clase que permitan saber cuáles son los valores `x` e `y` máximos que se han establecido en todos los puntos que se hayan creado hasta el momento.

Para indicar miembros de clase se usa la palabra clave static. En Punto, se podrían mantener los máximos globales de x e y observados y exponerlos mediante getters estáticos. Esto ilustra un estado compartido entre todas las instancias y una interfaz pública cuidadosa.

public class Punto {
    private double x;
    private double y;

    private static double maxX = Double.NEGATIVE_INFINITY;
    private static double maxY = Double.NEGATIVE_INFINITY;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
        actualizarMaximos(x, y);
    }

    private static void actualizarMaximos(double x, double y) {
        if (x > maxX) maxX = x;
        if (y > maxY) maxY = y;
    }

    public static double getMaxX() { return maxX; }
    public static double getMaxY() { return maxY; }

    // Resto de la clase (getX, getY, etc.)
}

## 14. Como sería un método factoría dentro de la clase `Punto` para construir un `Punto` a partir de dos coordenadas, pero que las redondee al entero más cercano. Escribe sólo el código del método, no toda la clase ¿Has usado `static`? 

public static Punto desdeRedondeado(double x, double y) {
    double xr = Math.round(x); // nearest integer (empate a +∞)
    double yr = Math.round(y);
    return new Punto(xr, yr);
}

Se ha usado static porque el método crea instancias nuevas sin requerir una instancia previa (es responsabilidad de la clase, no de un objeto particular). Como detalle, Math.round sigue la regla “.5 hacia arriba”; si se prefiere otra convención (p. ej., bankers rounding), podría emplearse Math.rint o BigDecimal con modo explícito.

## 15. Cambia la implementación de `Punto`. En vez de dos `double`, emplea un array interno de dos posiciones, intentando no modificar la interfaz pública de la clase.

public class Punto {
    private final double[] coords = new double[2]; // [0]=x, [1]=y

    public Punto(double x, double y) {
        coords[0] = x;
        coords[1] = y;
    }

    public double getX() { return coords[0]; }

    public double getY() { return coords[1]; }

    public double calcularDistanciaAOrigen() {
        double x = coords[0], y = coords[1];
        return Math.sqrt(x * x + y * y);
    }

    public double calcularDistanciaAPunto(Punto otro) {
        double dx = this.coords[0] - otro.coords[0];
        double dy = this.coords[1] - otro.coords[1];
        return Math.sqrt(dx * dx + dy * dy);
    }
}

La interfaz pública se mantiene: los mismos constructores y métodos. Solo cambia la representación interna, que continúa oculta gracias a private. Esto muestra la ventaja práctica de la encapsulación: se puede refactorizar la implementación sin afectar al código cliente.

## 16. Si un atributo va a tener un método "getter" y "setter" públicos, ¿no es mejor declararlo público? ¿Cuál es la convención más habitual sobre los atributos, que sean públicos o privados? ¿Tiene esto algo que ver con las "invariantes de clase"?

Aunque un atributo tenga getter y setter, no conviene hacerlo público. Con un campo público se perdería la capacidad de interponer validación o de evolucionar el acceso en el futuro (p. ej., notificar cambios, cachear, sincronizar), rompiendo la encapsulación.
La convención más habitual es declarar los atributos como private y exponer solo los métodos necesarios. Esto se relaciona directamente con las invariantes: centralizando la mutación en setters (o evitando mutación con inmutabilidad) se puede verificar que el estado se mantenga siempre válido.

## 17. ¿Qué significa que una clase sea **inmutable**? ¿qué es un método modificador? ¿Un método modificador es siempre un "setter"? ¿Tiene ventajas que una clase sea inmutable?

Una clase es inmutable si, una vez creada, su estado no cambia. No ofrece setters y cualquier “cambio” se expresa creando nuevas instancias (p. ej., String en Java). Un método modificador es cualquier método que altera el estado observable de la instancia; por tanto, no siempre coincide con un “setter” nominal (podría modificar varios atributos o hacerlo indirectamente).
La inmutabilidad aporta simplicidad de razonamiento, seguridad frente a aliasing, y una forma de thread‑safety sin bloqueos. Además, favorece el uso en colecciones y cachés. Su coste suele ser la creación de más objetos, que hoy día suele ser aceptable y, con diseño adecuado, eficiente.

## 18. ¿Es recomendable incluir métodos "setter" siempre y como convención?

No se recomienda añadir setters por defecto. Incluir setters indiscriminadamente amplía la superficie de mutación, dificulta mantener invariantes y puede permitir estados inválidos. El diseño orientado a objetos sugiere exponer solo lo que el modelo de dominio necesita realmente.
En muchos casos, es preferible la inmutabilidad o bien métodos modificadores intencionales de más alto nivel (p. ej., depositar(cantidad) en lugar de setSaldo). Esto protege las invariantes y hace más expresivo el contrato de la clase.

## 19. ¿La clase `String` en Java es mutable o inmutable? ¿Qué ocurre al concatenar dos cadenas? ¿Qué debemos hacer si vamos a hacer una operación que implique concatenar muchas veces para construir paso a paso una cadena muy larga?

String en Java es inmutable. Al concatenar, se crea un nuevo objeto String que contiene el resultado; las cadenas originales no cambian. En concatenaciones repetidas dentro de bucles, esta estrategia puede ser ineficiente por la creación de muchos objetos intermedios.
Para construir cadenas de forma incremental, se recomienda usar StringBuilder (no sincronizado, más rápido) o StringBuffer (sincronizado) y llamar a append(...). Al final, se convierte con toString(). Esto reduce costes de tiempo y memoria en operaciones intensivas.

## 20. En POO ¿Cómo se comparan objetos de una misma clase? ¿Por su contenido o por su identidad? ¿Qué es el método equals en Java? ¿Qué hace por defecto? ¿Cómo se deben comparar dos cadenas en Java? 

En POO se puede comparar por identidad (si son el mismo objeto) o por igualdad de contenido (si “representan lo mismo”). En Java, == compara identidad en referencias; para comparar contenido, se usa equals. Por defecto, equals en Object equivale a identidad (no compara campos).
Por tanto, las clases que deseen igualdad por valor deben sobrescribir equals (y coherentemente hashCode). En concreto, las cadenas en Java deben compararse con a.equals(b), no con ==, ya que == solo verifica si ambas referencias apuntan al mismo objeto.

## 21. ¿Qué son las clases "wrapper" en un lenguaje de programación orientado a objetos? ¿Cómo se hace? ¿Es un proceso automático? ¿Qué ventajas tienen? ¿Todos los lenguajes orientados a objetos tienen tipos primitivos y necesitan wrappers? 

Las clases wrapper encapsulan tipos primitivos en objetos: int → Integer, double → Double, etc. En Java, el autoboxing y unboxing realizan este envolvimiento/desenvolvimiento de forma automática en muchas situaciones (asignaciones, parámetros genéricos, etc.).
Sus ventajas incluyen usar colecciones genéricas (que requieren objetos), aprovechar métodos utilitarios y valores nulos (para representar ausencia). No todos los lenguajes necesitan wrappers: algunos (p. ej., Python) tratan todo como objetos; otros (p. ej., Java, C#) distinguen primitivos y objetos por rendimiento/semántica y, por ello, ofrecen wrappers.

## 22. ¿En POO qué es un **tipo de dato enumerado**? ¿En Java, un tipo de dato enumerado es una clase? ¿Qué ventajas tienen en términos de encapsulación los enumerados en Java?

Un tipo enumerado define un conjunto finito de valores posibles, expresivos y seguros en tiempo de compilación. En Java, enum es una clase especial con instancias predefinidas; puede tener campos privados, métodos, constructores (privados) y comportamiento.
En términos de encapsulación, los enum permiten ocultar detalles (p. ej., metadatos asociados a cada constante) tras una API clara, evitando valores mágicos y errores de validación. Además, garantizan que solo existan las instancias legales, reforzando invariantes de dominio.

## 23. Crea un tipo enumerado en Java que se llame `Mes`, con doce posibles instancias y que además proporcione métodos para obtener cuántos días tiene ese mes, el ordinal de ese mes en el año (1-12), empleando atributos privados y constructores del tipo enumerado. Añade además cuatro métodos para devolver si ese mes tiene algunos días de invierno, primavera, verano u otoño, indicando con un booleano el hemisferio (norte o sur, parámetro `enHemisferioNorte`). Es decir: `esDePrimavera(boolean esHemisferioNorte)`, `esDeVerano(boolean esHemisferioNorte)`, `esDeOtoño(boolean esHemisferioNorte)`, `esDeInvierno(boolean esHemisferioNorte)`

public enum Mes {
    ENERO(31, 1), FEBRERO(28, 2), MARZO(31, 3), ABRIL(30, 4),
    MAYO(31, 5), JUNIO(30, 6), JULIO(31, 7), AGOSTO(31, 8),
    SEPTIEMBRE(30, 9), OCTUBRE(31, 10), NOVIEMBRE(30, 11), DICIEMBRE(31, 12);

    private final int dias;
    private final int numero; // 1..12

    Mes(int dias, int numero) {
        this.dias = dias;
        this.numero = numero;
    }

    public int getDias() {
        return dias;
    }

    public int getNumeroMes() {
        return numero;
    }

    public boolean esDePrimavera(boolean enHemisferioNorte) {
        return enHemisferioNorte ? (this == MARZO || this == ABRIL || this == MAYO)
                                 : (this == SEPTIEMBRE || this == OCTUBRE || this == NOVIEMBRE);
    }

    public boolean esDeVerano(boolean enHemisferioNorte) {
        return enHemisferioNorte ? (this == JUNIO || this == JULIO || this == AGOSTO)
                                 : (this == DICIEMBRE || this == ENERO || this == FEBRERO);
    }

    public boolean esDeOtono(boolean enHemisferioNorte) {
        return enHemisferioNorte ? (this == SEPTIEMBRE || this == OCTUBRE || this == NOVIEMBRE)
                                 : (this == MARZO || this == ABRIL || this == MAYO);
    }

    public boolean esDeInvierno(boolean enHemisferioNorte) {
        return enHemisferioNorte ? (this == DICIEMBRE || this == ENERO || this == FEBRERO)
                                 : (this == JUNIO || this == JULIO || this == AGOSTO);
    }
}

Se fijan 28 días para febrero (no bisiesto) por simplicidad; si se requiere exactitud con años bisiestos, podría añadirse lógica externa o un método que reciba el año. Se usa un constructor privado (implícitamente privado en enum) y atributos privados para encapsular metadatos del mes, y se exponen métodos públicos que conforman su interfaz.