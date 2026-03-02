<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Excepciones". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 3. Excepciones

## 1. Empecemos un tema sobre control de errores en lenguajes de programación, con algo básico. En C, donde no existen las excepciones, pongamos un ejemplo de una raíz que toma número flotante positivo. Queremos controlar el error si la función recibe un número negativo. El usuario debe ser informado pero desde fuera de la función `raiz` ¿Cómo indicamos ese error?. Enumera dos opciones diferentes de diseñar, poniendo un ejemplo de código de cada una.

En el lenguaje C, al no existir un mecanismo de excepciones, se recurre a la inspección manual de estados. Dos opciones comunes para informar de un error en la función raiz sin imprimir mensajes dentro de ella son:

Opción A: Uso de un valor de retorno especial. Se reserva un valor que no pertenece al dominio de resultados válidos (como un número negativo o NAN) para indicar el fallo. El llamador debe comparar el resultado con dicho valor mediante un if.

Opción B: Uso de un puntero de estado o variable global. Se pasa un argumento adicional por referencia (puntero) que la función modifica para indicar éxito o fracaso, o bien se utiliza la variable global errno.

// Opción A: Valor de retorno especial
float raizA(float n) {
    if (n < 0) return -1.0; // Valor centinela
    return sqrt(n);
}

// Opción B: Puntero de estado (similar a pasar por referencia)
float raizB(float n, int *error) {
    if (n < 0) {
        *error = 1;
        return 0;
    }
    *error = 0;
    return sqrt(n);
}

## 2. Brevemente ¿Qué es una **"excepción"**? ¿Con qué objetivo las usa un programador cuando implementa funciones o cuando las llama?

Una excepción es un evento que ocurre durante la ejecución de un programa y que interrumpe el flujo normal de sus instrucciones. Técnicamente, es un mecanismo que permite a una función "rendirse" ante una situación que no sabe o no puede resolver (como una precondición violada), delegando la responsabilidad de la decisión a un nivel superior en la jerarquía de llamadas.

El objetivo del programador al implementarlas es garantizar la robustez. Al llamar a una función, el programador las usa para centralizar la gestión de errores en un solo punto, evitando llenar la lógica de negocio con constantes comprobaciones de if-else que oscurecen el propósito real del algoritmo.

## 3. Reescribe el mismo ejemplo de raiz, pero en Java, metiendo ese método en una clase `Calculadora` y llama a dicho método desde el método `main`, mostrando cómo se puede controlar desde fuera.

En Java, el error se gestiona lanzando una instancia de una clase de excepción. El método main actúa como el entorno que decide cómo reaccionar ante el fallo del objeto Calculadora.

public class Calculadora {
    public double raiz(double n) {
        if (n < 0) {
            throw new IllegalArgumentException("No se puede calcular la raíz de un negativo");
        }
        return Math.sqrt(n);
    }

    public static void main(String[] args) {
        Calculadora calc = new Calculadora();
        try {
            System.out.println(calc.raiz(-5));
        } catch (IllegalArgumentException e) {
            System.out.println("Error capturado en main: " + e.getMessage());
        }
    }
}

## 4. ¿Qué es **"lanzar"** una excepción? ¿Qué es **"controlar"** o **"capturar"** una excepción? ¿Qué es que se **"propague"** una excepción? ¿Qué le va ocurriendo a las funciones en la pila de llamadas por donde se va propagando la excepción? ¿Las funciones que no la controlan se reanudan después de alguna forma? Explica con el mismo ejemplo anterior en Java de la raíz cuadrada.

Lanzar (throw) es el acto de crear el objeto excepción y detener la ejecución de la función actual. Capturar o controlar es el uso de un bloque catch para detener ese objeto y ejecutar un código de recuperación. La propagación es el viaje que realiza la excepción hacia atrás a través de la pila de llamadas (stack) buscando un bloque que la capture.

Cuando una excepción se propaga, las funciones en la pila de llamadas finalizan abruptamente. No se ejecutan las líneas de código que están por debajo de la llamada que falló. Las funciones que no controlan la excepción no se reanudan; mueren definitivamente y el control pasa a la función que las invocó, y así sucesivamente hasta llegar al main o, si nadie la captura, al sistema operativo (provocando el cierre del programa).

## 5. ¿Qué ventajas tiene frente a C, la **"propagación natural"** de las excepciones a través de la pila (*stack*) de llamadas?

La ventaja principal frente a C es la limpieza del código y la seguridad. En C, si una función a diez niveles de profundidad falla, cada una de las nueve funciones intermedias debe recibir el error, comprobarlo y devolverlo manualmente al nivel superior. Es muy fácil que un programador olvide un if intermedio, dejando al programa en un estado inconsistente o "zombie".

En Java, la propagación es automática y obligatoria. Los niveles intermedios que no tienen nada que aportar a la solución del error simplemente se ignoran, y la excepción "salta" directamente hasta el manejador adecuado. Esto asegura que ningún error pase desapercibido por negligencia en el flujo de retorno.

## 6. En orientación a objetos, ¿las excepciones suelen ser objetos? ¿Qué ventajas tiene esto en términos de encapsulación? ¿Podemos entonces crear excepciones personalizadas?

En POO, las excepciones son objetos que pertenecen a una jerarquía de clases. Esto permite aplicar la encapsulación: la excepción no solo dice "algo falló", sino que puede contener atributos privados, métodos para obtener detalles técnicos o incluso soluciones sugeridas, todo empaquetado dentro del objeto.

Gracias a la herencia, se pueden crear excepciones personalizadas extendiendo clases existentes (como Exception o RuntimeException). Esto permite que el sistema de tipos diferencie errores de negocio (ej. SaldoInsuficienteException) de errores técnicos, permitiendo capturas selectivas y una organización mucho más lógica que los códigos numéricos de C.

## 7. En relación con las ventajas de la encapsulación, comparando el ejemplo en C con Java. ¿Qué **información esencial** lleva cualquier **objeto excepción** que es muy útil tener cuando se llega a un manejador?

Comparado con un simple entero de error en C, un objeto excepción en Java transporta información vital: el mensaje de error (una cadena descriptiva), el tipo de excepción (identificado por el nombre de la clase) y, lo más importante, el Stack Trace (traza de la pila).

La traza de la pila es una lista de todos los métodos y líneas de código exactas por las que pasó el programa antes de fallar. Tener este "mapa" en el manejador permite realizar un diagnóstico preciso del contexto del error, algo que en C requeriría herramientas externas de depuración o un sistema de log manual muy complejo.

## 8. En Java, sobre el bloque **"try-catch"**, ¿se pueden tener más de un bloque `catch`? ¿cuántos bloques `catch` se ejecutan?

Sí, se pueden tener múltiples bloques catch para un solo bloque try. Esto permite dar un tratamiento específico a cada tipo de problema. Por ejemplo, se puede querer reintentar una operación si el error es de red, pero cerrar el programa si el error es de permisos de disco.

Sin embargo, en cada ejecución del bloque try, solo se ejecuta un bloque catch. El entorno de ejecución de Java busca de arriba hacia abajo el primer catch cuya clase sea compatible con la excepción lanzada. Una vez que se entra en un bloque catch, los demás se ignoran y la ejecución continúa después de la estructura completa.

## 9. Si las excepciones producen rupturas en el código llamador, ¿cómo podemos garantizar que se ejecuta siempre finalmente un código necesario para cierre de ficheros, liberacion de recursos, antes de que continúe propagándose la excepción? Pon un ejemplo en Java con `finally`, tanto con `catch` como sin él.

El bloque finally garantiza la ejecución de código de limpieza. Es fundamental para liberar recursos que no gestiona el Garbage Collector de forma inmediata, como punteros a archivos o sockets.

// Ejemplo con catch y finally
try {
    abrirArchivo();
    leerDatos();
} catch (IOException e) {
    System.out.println("Error de lectura");
} finally {
    cerrarArchivo(); // Se ejecuta SIEMPRE
}

// Ejemplo sin catch (solo para asegurar limpieza y dejar propagar)
try {
    operacionPeligrosa();
} finally {
    liberarMemoria(); // Se ejecuta antes de que la excepción siga su camino
}

## 10. En Java, el bloque `finally` puede ir sin `catch`? ¿Se ejecuta siempre tanto si ocurre como si no ocurre una excepción? ¿Y si hay un `return` en medio del `try`?

El bloque finally puede ir sin catch (formando un try-finally). Su propósito en ese caso no es gestionar el error, sino asegurar que el código de limpieza se ejecute antes de que la excepción continúe propagándose hacia arriba.

Se ejecuta siempre, ocurra o no una excepción. Incluso si hay un return dentro del bloque try, el bloque finally se ejecuta justo después del return pero antes de que el método entregue realmente el control al llamador. Es una garantía absoluta de ejecución.

## 11. En Java, qué son las excepciones **"controladas"** y las **"no controladas"**? ¿Qué papel juega `RuntimeException`? Pon un ejemplo de excepciones típicas controladas y no controladas que incluso nosotros mismos podríamos usar. Haz dos listas con 3 o 4 ejemplos de situación donde se suele preferir una excepción controlada y donde se suele preferir una excepción no controlada.

Las excepciones controladas (descendientes de Exception) son aquellas que el compilador obliga a gestionar o declarar. Las no controladas (descendientes de RuntimeException) representan errores de programación o condiciones irrecuperables que el compilador no obliga a verificar.

Uso de Controladas (Checked),Uso de No Controladas (Unchecked)
Fallo al conectar a una DB externa.,División por cero (ArithmeticException).
Archivo no encontrado (FileNotFoundException).,Acceso a puntero nulo (NullPointerException).
Interrupción de un hilo de ejecución.,Índice de array fuera de rango.
Error de formato en un mensaje de red.,Argumento de método inválido.

Se prefiere la controlada cuando el error es una contingencia razonable que el llamador debería prever. Se prefiere la no controlada cuando el error indica un fallo en la lógica del programador (un bug) que debería corregirse en el código, no "gestionarse".

## 12. ¿Qué es y para qué se usa `throws`? ¿Por qué es alternativa a capturar una excepción controlada?

La palabra clave throws se utiliza en la firma de un método para declarar que dicho método puede lanzar ciertas excepciones sin capturarlas. Es una forma de avisar al programador que use ese método: "Ten cuidado, esto puede fallar y tú eres responsable de decidir qué hacer".

Es la alternativa a capturar la excepción porque permite la delegación. Si un método no sabe cómo solucionar un error (por ejemplo, un método que lee un archivo no sabe si debe pedir otro nombre al usuario o cerrar el programa), usa throws para pasarle "la patata caliente" al nivel superior.

## 13. Pon un ejemplo en Java de firma de método que incluya `throws`, de una función que abre un fichero pero que declara que no le interesa menejar la excepción de si el fichero no existe, sino que se propague hacia arriba. Eso sí, acuérdate del `finally`.

En este ejemplo, el método procesar delega la responsabilidad del error, pero cumple con su deber de cerrar el recurso

public void procesarArchivo(String ruta) throws FileNotFoundException {
    Scanner sc = null;
    try {
        sc = new Scanner(new File(ruta));
        // ... lógica ...
    } finally {
        if (sc != null) sc.close(); 
        System.out.println("Recurso liberado.");
    }
}

## 14. ¿Podemos poner en `throws` excepciones no controladas, como `RuntimeException`? ¿Debería el método llamador entonces poner `try-catch` en ese caso? ¿Qué sentido tendría?

Es técnicamente posible poner excepciones no controladas en el throws, pero es innecesario e inusual. El compilador no lo requiere y no obliga al llamador a usar try-catch.

Su único sentido sería meramente documentativo, para advertir explícitamente a otros programadores que esa función es propensa a un error específico (como un fallo de validación), aunque no estén obligados por el lenguaje a gestionarlo.

## 15. ¿Cuándo se recomienda usar excepciones controladas, como `IOException`, y cuándo no controladas como `IllegalArgumentException`? ¿Existen en todos los lenguajes ambas opciones? En los que sólo existe una opción, ¿cuál es la más habitual?

Se recomienda usar excepciones controladas para condiciones externas al programa (red, archivos, bases de datos) y no controladas para violaciones de contratos o errores lógicos internos.

Java es prácticamente el único lenguaje masivo que implementa excepciones controladas de forma estricta. Otros lenguajes modernos como C#, Python o C++ solo tienen excepciones "no controladas" (en el sentido de que el compilador nunca te obliga a capturarlas), siendo esta la opción más habitual en la industria actualmente por considerar que las controladas ensucian demasiado la firma de los métodos.

## 16. ¿Tiene sentido lanzar excepciones dentro del `catch`? ¿Se puede relanzar la misma excepción capturada? ¿Cuándo tendría sentido hacer esto último? Pon ejemplos de ambos casos.

Es perfectamente válido lanzar una excepción dentro de un catch. Se puede relanzar la misma excepción capturada (throw e;) o lanzar una nueva.

Relanzar la misma tiene sentido cuando el método quiere realizar una acción parcial (como registrar el error en un log o liberar un recurso local) pero no quiere "curar" el error, permitiendo que siga subiendo hasta el usuario final. Lanzar una nueva suele hacerse para cambiar el tipo de excepción a uno más adecuado al contexto del llamador.

## 17. ¿En qué consiste que una excepción sea la **"causa"** de otra excepción? Pon un ejemplo en Java, donde capturemos una excepción de bajo nivel y la encapsulemos en otra personalizada de alto nivel. Cuando una excepción sale por pantalla y tiene una causa, ¿se ve?

Que una excepción sea la causa de otra significa que el error original se envuelve dentro de una nueva excepción. Esto permite mantener la abstracción: un método de "Persistencia" puede capturar un error técnico de SQL y lanzar una ErrorDeAlmacenamientoException que guarde la excepción original en su interior.

En Java, esto se hace pasando la excepción original al constructor de la nueva. Cuando la excepción sale por pantalla, el Stack Trace muestra claramente el mensaje "Caused by:" seguido de la excepción original, permitiendo ver toda la cadena de fallos desde el nivel más alto hasta la raíz del problema.

try {
    // Código que lanza SQLException
} catch (SQLException e) {
    throw new MiExcepcionPersonalizada("Error de negocio", e); // 'e' es la causa
}
