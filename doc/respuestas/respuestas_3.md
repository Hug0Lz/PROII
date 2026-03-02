# TEMA 3. Excepciones

## 1. Empecemos un tema sobre control de errores en lenguajes de programación, con algo básico. En C, donde no existen las excepciones, pongamos un ejemplo de una raíz que toma número flotante positivo. Queremos controlar el error si la función recibe un número negativo. El usuario debe ser informado pero desde fuera de la función `raiz` ¿Cómo indicamos ese error?. Enumera dos opciones diferentes de diseñar, poniendo un ejemplo de código de cada una.

### Respuesta

En el lenguaje C, al carecer de un mecanismo nativo de excepciones, el control de errores debe realizarse de forma manual y explícita. La responsabilidad recae íntegramente en el programador, quien debe definir una convención para comunicar que algo ha fallado. El objetivo es siempre evitar que el programa continúe su ejecución con datos inválidos que podrían provocar un comportamiento errático o un cierre inesperado.

La primera opción consiste en reservar el valor de retorno para un código de estado (normalmente un entero) y pasar el resultado real a través de un puntero. Si la función recibe un número negativo, devuelve un código de error (como -1) y no modifica el valor apuntado. Esto obliga al llamador a comprobar siempre el valor devuelto antes de proceder.

```
int raiz(double n, double *resultado) {
    if (n < 0) return -1; // Código de error
    *resultado = sqrt(n);
    return 0; // Éxito
}
```
La segunda opción es devolver un valor especial que represente un "no-número" (NaN) o un valor centinela fuera del rango posible. En este caso, el llamador debe conocer qué valor indica el error y realizar la comprobación. Aunque es más directo, presenta el riesgo de que el programador olvide realizar la validación y use el valor erróneo en cálculos posteriores.

```
double raiz(double n) {
    if (n < 0) return -1.0; // Valor centinela (la raíz nunca es negativa)
    return sqrt(n);
}
```

## 2. Brevemente ¿Qué es una **"excepción"**? ¿Con qué objetivo las usa un programador cuando implementa funciones o cuando las llama?

### Respuesta

Una excepción es un evento excepcional que ocurre durante la ejecución de un programa y que interrumpe el flujo normal de las instrucciones. Técnicamente, en lenguajes como Java, se materializa como un objeto que encapsula información sobre el error (qué ocurrió, dónde y en qué estado estaba el programa). A diferencia de los códigos de error en C, una excepción "salta" fuera del flujo lineal de la función.

El objetivo principal de usar excepciones es separar la lógica principal del programa de la gestión de errores. Al implementar una función, el programador lanza una excepción para delegar la responsabilidad del error a quien tenga suficiente contexto para decidir qué hacer. Esto evita "ensuciar" el algoritmo principal con múltiples estructuras condicionales que solo comprueban fallos.

Desde el punto de vista de quien llama a la función, las excepciones permiten centralizar la captura de errores. En lugar de comprobar el éxito de cada pequeña operación, se puede agrupar un conjunto de llamadas y gestionar cualquier fallo que ocurra en ellas de manera unificada, mejorando la legibilidad y la robustez del software.


## 3. Reescribe el mismo ejemplo de raiz, pero en Java, metiendo ese método en una clase `Calculadora` y llama a dicho método desde el método `main`, mostrando cómo se puede controlar desde fuera.

### Respuesta

En Java, la gestión se vuelve mucho más estructurada. La función ya no devuelve un código de error, sino que interrumpe su ejecución mediante la palabra reservada throw. Esto garantiza que, si el dato es inválido, el método nunca devolverá un resultado parcial o incorrecto.
```
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
            double resultado = calc.raiz(-5.0);
            System.out.println("El resultado es: " + resultado);
        } catch (IllegalArgumentException e) {
            System.out.println("Error detectado: " + e.getMessage());
        }
    }
}
```
Como se observa en el ejemplo, el método main envuelve la llamada en un bloque try. Si la excepción es lanzada dentro de raiz, el flujo salta inmediatamente al bloque catch, ignorando la línea donde se imprimiría el resultado. Esta estructura permite que el error sea gestionado fuera de la función que lo detecta, cumpliendo con el requisito de informar al usuario desde el nivel superior.


## 4. ¿Qué es **"lanzar"** una excepción? ¿Qué es **"controlar"** o **"capturar"** una excepción? ¿Qué es que se **"propague"** una excepción? ¿Qué le va ocurriendo a las funciones en la pila de llamadas por donde se va propagando la excepción? ¿Las funciones que no la controlan se reanudan después de alguna forma? Explica con el mismo ejemplo anterior en Java de la raíz cuadrada.

### Respuesta

Lanzar (throw) es el acto de señalar que ha ocurrido un error. En el ejemplo, cuando la función detecta un valor negativo, crea un objeto de excepción y lo "lanza". En ese instante, la ejecución de la función raiz se detiene por completo; no se ejecuta ningún return ni ninguna instrucción posterior.

Capturar (catch) es la acción de interceptar una excepción que ha sido lanzada para procesarla. Se realiza mediante un manejador que detiene la subida de la excepción por la pila de llamadas. Al capturarla, el programa no termina de forma abrupta, sino que ejecuta el código de recuperación previsto y permite que la ejecución continúe después del bloque try-catch.

Propagar es lo que ocurre cuando una función no captura una excepción que se ha producido en su interior (o en una función llamada por ella). La excepción viaja hacia atrás por la pila de llamadas (call stack). En cada paso, la función actual se interrumpe y se elimina de la pila hasta encontrar un bloque catch. Si llega al método main y nadie la captura, el programa finaliza y muestra el error. Las funciones que no la controlan nunca se reanudan; se cierran definitivamente en el punto donde se produjo la interrupción.


## 5. ¿Qué ventajas tiene frente a C, la **"propagación natural"** de las excepciones a través de la pila (*stack*) de llamadas?

### Respuesta

La principal ventaja es la eliminación del código redundante de comprobación. En C, si una función A llama a B, y B llama a C, y C detecta un error, tanto B como A deben incluir instrucciones if para capturar el error de la función inferior y devolverlo a la superior. Esto oscurece la lógica del programa con una cadena interminable de validaciones de códigos de retorno.

Con la propagación natural, el error "flota" automáticamente hacia arriba. El programador solo escribe el código de gestión de errores en el lugar donde realmente puede hacer algo útil para solucionarlo. Los niveles intermedios de la pila de llamadas no necesitan saber que el error existe; simplemente se detienen, permitiendo un diseño mucho más limpio y enfocado en el caso de éxito.

Otra ventaja crucial es que las excepciones no se pueden ignorar por descuido. En C, es muy fácil olvidar comprobar el valor de retorno de una función, lo que permite que el error se propague silenciosamente y cause fallos mucho más difíciles de depurar más adelante. En Java, una excepción no capturada detendrá el programa, obligando al desarrollador a ser consciente de la existencia de posibles fallos y a decidir cómo manejarlos.


## 6. En orientación a objetos, ¿las excepciones suelen ser objetos? ¿Qué ventajas tiene esto en términos de encapsulación? ¿Podemos entonces crear excepciones personalizadas?

### Respuesta

Continuamos con el segundo bloque de respuestas (preguntas 6 a 10) sobre el Tema 3: Excepciones.

6. En orientación a objetos, ¿las excepciones suelen ser objetos? ¿Qué ventajas tiene esto en términos de encapsulación? ¿Podemos entonces crear excepciones personalizadas?
Respuesta
En los lenguajes orientados a objetos como Java, las excepciones son efectivamente objetos. Específicamente, son instancias de clases que heredan de una jerarquía común (en Java, la clase base es Throwable). Esto supone un cambio drástico respecto a C, donde un error suele ser un simple número entero. Al ser objetos, las excepciones pueden contener no solo un mensaje, sino también un estado complejo y un comportamiento asociado.

Desde el punto de vista de la encapsulación, que las excepciones sean objetos permite agrupar toda la información relevante del error dentro de la propia entidad. Se pueden incluir códigos de error internos, la hora en que ocurrió el fallo, el nombre del recurso que falló o incluso métodos para formatear el error de una forma específica. Toda esta complejidad queda oculta tras la interfaz del objeto excepción, entregando al manejador una herramienta potente y fácil de usar.

Gracias a esta naturaleza, es perfectamente posible y recomendable crear excepciones personalizadas. Basta con definir una nueva clase que herede de Exception (o alguna de sus subclases). Esto permite que el programador defina tipos de error específicos para su dominio de negocio, como SaldoInsuficienteException o UsuarioNoEncontradoException, lo que dota al código de una semántica mucho más clara y profesional que el uso de códigos numéricos genéricos.


## 7. En relación con las ventajas de la encapsulación, comparando el ejemplo en C con Java. ¿Qué **información esencial** lleva cualquier **objeto excepción** que es muy útil tener cuando se llega a un manejador?

### Respuesta

A diferencia de un código de error en C, que es una información "plana" y carente de contexto, un objeto excepción en Java transporta una pieza de información fundamental: la traza de la pila (stack trace). Este es un informe detallado que indica la secuencia exacta de llamadas a métodos que se estaban ejecutando en el momento preciso en que se lanzó la excepción, incluyendo los nombres de los archivos y los números de línea.

Esta información es vital para la depuración, ya que permite reconstruir el camino que llevó al fallo sin necesidad de insertar múltiples trazas de impresión manuales. Además de la traza, el objeto suele llevar un mensaje descriptivo y, en ocasiones, una referencia a otra excepción previa que causó el problema actual (encadenamiento de excepciones), permitiendo entender la raíz del fallo en sistemas con múltiples capas.

Otra ventaja de la encapsulación es el tipo de la excepción. El simple hecho de que el objeto sea de una clase específica (por ejemplo, FileNotFoundException) ya proporciona información semántica inmediata al programador. En C, habría que consultar un manual o un fichero de cabecera para saber qué significa el error 2, mientras que en Java el propio nombre de la clase del objeto describe la naturaleza del problema.


## 8. En Java, sobre el bloque **"try-catch"**, ¿se pueden tener más de un bloque `catch`? ¿cuántos bloques `catch` se ejecutan?

### Respuesta

En Java, es posible y habitual encadenar múltiples bloques catch después de un único bloque try. Esto se hace con el objetivo de gestionar de manera diferenciada distintos tipos de errores que podrían ocurrir en el mismo fragmento de código. Por ejemplo, en un bloque que lee un archivo y procesa números, se podría querer capturar por separado un error de lectura (IOException) de un error de formato numérico (NumberFormatException).

Sin embargo, a pesar de tener varios bloques definidos, solo se ejecuta un único bloque catch por cada excepción lanzada: aquel que sea compatible con el tipo del objeto lanzado y que aparezca primero en el orden de escritura. Una vez que se encuentra un bloque que coincide con la excepción, el código de ese bloque se ejecuta y el programa salta directamente al final de toda la estructura try-catch.

Es fundamental tener en cuenta la jerarquía de clases al ordenar los bloques. Si se coloca un catch de una clase muy general (como Exception) antes que uno de una clase más específica, el primero "atrapará" todas las excepciones y los bloques siguientes quedarán inaccesibles. El compilador de Java detecta este error y obliga a ordenar los bloques de la captura desde la excepción más específica a la más general.


## 9. Si las excepciones producen rupturas en el código llamador, ¿cómo podemos garantizar que se ejecuta siempre finalmente un código necesario para cierre de ficheros, liberacion de recursos, antes de que continúe propagándose la excepción? Pon un ejemplo en Java con `finally`, tanto con `catch` como sin él.

### Respuesta

Para garantizar la ejecución de código de limpieza (como cerrar ficheros, liberar memoria o cerrar conexiones de red) independientemente de si ocurre un error o no, se utiliza el bloque finally. En C, esto suele requerir repetir el código de liberación en cada punto de salida de la función o usar etiquetas goto, lo cual es propenso a errores. En Java, el bloque finally se coloca al final de la estructura y el sistema garantiza su ejecución.

A continuación se muestra un ejemplo donde se intenta realizar una operación y se garantiza el cierre de un recurso ficticio, tanto si hay éxito como si hay error:
```
try {
    recurso.abrir();
    recurso.procesar(); // Podría lanzar una excepción
} catch (Exception e) {
    System.out.println("Error procesando: " + e.getMessage());
} finally {
    // Este código se ejecuta SIEMPRE
    recurso.cerrar();
    System.out.println("Recurso liberado correctamente.");
}
```
También es posible utilizarlo sin el bloque catch. Esto es útil cuando no se quiere gestionar el error en ese punto (se prefiere que se propague hacia arriba), pero se tiene la responsabilidad de dejar los recursos limpios antes de que la función se interrumpa y se elimine de la pila de llamadas.

## 10. En Java, el bloque `finally` puede ir sin `catch`? ¿Se ejecuta siempre tanto si ocurre como si no ocurre una excepción? ¿Y si hay un `return` en medio del `try`?

### Respuesta

Sí, en Java es perfectamente válido escribir una estructura try-finally sin incluir ningún bloque catch. Esta combinación se utiliza cuando la función actual no tiene la capacidad o la responsabilidad de solucionar el error, permitiendo que la excepción se propague a la función llamadora, pero asegurando que antes de "abandonar" la función se ejecuten tareas críticas de limpieza.

La característica más potente de finally es su persistencia: se ejecuta siempre, incluso si existe una instrucción return dentro del bloque try o del bloque catch. Cuando el programa llega a un return, el valor a devolver se reserva temporalmente, pero antes de que la función termine efectivamente, el control salta al bloque finally. Una vez terminado este, la función devuelve el valor y se cierra.

Solo existen situaciones extremadamente raras donde finally no se ejecuta, como una caída del sistema, un corte de energía o la llamada explícita a System.exit(), que detiene la Máquina Virtual de Java inmediatamente. Para la lógica de programación habitual, se debe considerar que el bloque finally es un "seguro de vida" que el lenguaje ofrece para mantener la integridad de los recursos del sistema.


## 11. En Java, qué son las excepciones **"controladas"** y las **"no controladas"**? ¿Qué papel juega `RuntimeException`? Pon un ejemplo de excepciones típicas controladas y no controladas que incluso nosotros mismos podríamos usar. Haz dos listas con 3 o 4 ejemplos de situación donde se suele preferir una excepción controlada y donde se suele preferir una excepción no controlada.

### Respuesta

En Java, las excepciones se dividen en dos grandes categorías según cómo el compilador obliga a tratarlas. Las excepciones controladas (checked exceptions) son aquellas que el lenguaje obliga a gestionar explícitamente; si un método puede lanzarlas, el programador debe capturarlas con un try-catch o declararlas con throws, de lo contrario, el código no compilará. Representan condiciones externas al programa que son razonablemente previsibles, como que un archivo no exista o un fallo de red.

Las excepciones no controladas (unchecked exceptions) son aquellas que heredan de la clase RuntimeException. A diferencia de las anteriores, el compilador no obliga a capturarlas ni a declararlas. Estas suelen representar errores de lógica del programador o "bugs" que no deberían ocurrir si el código estuviera correctamente escrito, como acceder a un índice de array inexistente o realizar una división por cero.

Ejemplos típicos:

Controladas: IOException, SQLException, FileNotFoundException.

No controladas: NullPointerException, ArrayIndexOutOfBoundsException, ArithmeticException.

Situaciones de uso sugeridas:
Se suele preferir una excepción controlada cuando:

El error depende de factores externos (ficheros, base de datos).

El programa puede recuperarse del error (por ejemplo, pidiendo otra ruta de archivo).

Se quiere obligar al usuario de la función a ser consciente del riesgo.

La condición de error es parte de la "interfaz" normal de la función.

Se prefiere una excepción no controlada cuando:

El error es un fallo de programación (parámetros inválidos).

La recuperación es imposible y el programa debe detenerse.

El error puede ocurrir en casi cualquier parte (como un puntero nulo).

Se quiere mantener el código limpio de excesivos bloques try-catch.


## 12. ¿Qué es y para qué se usa `throws`? ¿Por qué es alternativa a capturar una excepción controlada?

### Respuesta

La palabra reservada throws se utiliza en la firma o declaración de un método para indicar que dicho método puede lanzar una o varias excepciones específicas. Funciona como un aviso para quien llame al método: "ten cuidado, este proceso puede fallar de esta manera y tú serás responsable de manejarlo". Es, en esencia, una forma de documentar formalmente los riesgos de una función.

Es la alternativa directa a capturar la excepción con un bloque try-catch dentro de la propia función. En lugar de intentar solucionar el problema en ese momento, el método decide delegar la responsabilidad. Al usar throws, la excepción se propaga automáticamente hacia el llamador, permitiendo que el error suba por la pila de llamadas hasta encontrar un punto donde exista el contexto suficiente para gestionarlo adecuadamente.

Esta opción es especialmente útil en arquitecturas por capas. Por ejemplo, una función de bajo nivel que lee datos de un sensor en C no sabría cómo informar al usuario final; en Java, simplemente declara un throws y deja que sea la capa de interfaz de usuario la que capture el error y muestre un mensaje comprensible, manteniendo las responsabilidades de cada parte del código bien separadas.


## 13. Pon un ejemplo en Java de firma de método que incluya `throws`, de una función que abre un fichero pero que declara que no le interesa menejar la excepción de si el fichero no existe, sino que se propague hacia arriba. Eso sí, acuérdate del `finally`.

### Respuesta

En este ejemplo, el método leerConfiguracion intenta abrir un archivo. Como no desea gestionar el posible error de que el fichero no exista, utiliza throws para delegar el problema al método main. Se incluye un bloque finally para asegurar la limpieza de recursos.
```
import java.io.*;

public class GestorArchivos {
    public void leerConfiguracion(String ruta) throws FileNotFoundException {
        FileReader fr = null;
        try {
            fr = new FileReader(ruta);
            // Simulación de procesamiento
            System.out.println("Archivo abierto correctamente.");
        } finally {
            // El bloque finally se ejecuta antes de que la excepción se propague
            if (fr != null) {
                try { fr.close(); } catch (IOException e) { /* Ignorado por brevedad */ }
                System.out.println("Recurso cerrado en finally.");
            }
        }
    }

    public static void main(String[] args) {
        GestorArchivos gestor = new GestorArchivos();
        try {
            gestor.leerConfiguracion("config.txt");
        } catch (FileNotFoundException e) {
            System.out.println("Error en el main: El archivo no se encontró.");
        }
    }
}
```


## 14. ¿Podemos poner en `throws` excepciones no controladas, como `RuntimeException`? ¿Debería el método llamador entonces poner `try-catch` en ese caso? ¿Qué sentido tendría?

### Respuesta

Técnicamente, sí es posible incluir excepciones no controladas (hijas de RuntimeException) en la cláusula throws de un método. El compilador de Java lo permite sin generar errores. Sin embargo, al ser no controladas, el compilador no obligará al método llamador a usar un bloque try-catch ni a volver a declararlas. La propagación ocurrirá de forma natural se declare o no en la firma.

El principal sentido de hacerlo es la documentación y claridad. Al incluir una RuntimeException en la firma, se está avisando explícitamente a otros programadores sobre condiciones críticas que podrían hacer fallar la función. Es una forma de decir: "aunque no te obligue el compilador, quiero que sepas que esto puede lanzar un IllegalArgumentException si me pasas datos incorrectos".

No obstante, en la práctica habitual de Java, se prefiere documentar estas excepciones mediante etiquetas Javadoc (@throws) en lugar de incluirlas en la firma del método. Esto mantiene la firma limpia para las excepciones que realmente requieren una acción obligatoria por parte del llamador, reservando la declaración throws para las excepciones controladas que definen el contrato estricto del método.



## 15. ¿Cuándo se recomienda usar excepciones controladas, como `IOException`, y cuándo no controladas como `IllegalArgumentException`? ¿Existen en todos los lenguajes ambas opciones? En los que sólo existe una opción, ¿cuál es la más habitual?

### Respuesta

La recomendación general es usar excepciones controladas para condiciones de las que el programa puede y debe intentar recuperarse (errores de E/S, red, etc.). Se deben usar excepciones no controladas para errores de programación que no deberían suceder en un sistema correcto (punteros nulos, argumentos ilegales, estados inconsistentes del objeto). Si el error indica que el programador ha usado mal la API, debe ser no controlada.

Java es un lenguaje relativamente único en esta distinción. La mayoría de los lenguajes modernos orientados a objetos, como C++, Python, C# o Kotlin, solo disponen de excepciones no controladas. En estos lenguajes, ninguna excepción es verificada por el compilador en tiempo de compilación; el programador tiene la libertad (y la responsabilidad) de decidir qué capturar y qué dejar propagar.

En los lenguajes donde solo existe una opción, el modelo más habitual es el de las excepciones no controladas. Esto evita el problema del "atrapado vacío" (programadores que capturan una excepción solo para que el código compile, pero dejan el bloque catch vacío), una crítica común al sistema de excepciones controladas de Java. En estos lenguajes, el manejo de errores depende más de una buena documentación y de la disciplina del desarrollador.


## 16. ¿Tiene sentido lanzar excepciones dentro del `catch`? ¿Se puede relanzar la misma excepción capturada? ¿Cuándo tendría sentido hacer esto último? Pon ejemplos de ambos casos.

### Respuesta

Es una práctica común y con mucho sentido técnico lanzar una excepción dentro de un bloque catch. Esto ocurre normalmente cuando se desea transformar una excepción técnica o de bajo nivel en una excepción con mayor significado para la lógica de negocio. Al capturar el error original, se puede realizar alguna acción inmediata (como cerrar un log o liberar un recurso) y luego lanzar una nueva excepción que sea más fácil de entender para las capas superiores del programa.

También es posible relanzar la misma excepción que se acaba de capturar. Esto se conoce como rethrowing. En este caso, el bloque catch actúa como un punto de inspección: se captura la excepción para registrar el error en un archivo de log o para realizar una limpieza parcial de la memoria, pero como el método actual no puede solucionar el problema de fondo, vuelve a lanzar la misma instancia del objeto excepción para que siga su camino por la pila de llamadas.

```
// Ejemplo 1: Lanzar una nueva excepción (Traducción)
try {
    accederBaseDatos();
} catch (SQLException e) {
    // Se transforma un error técnico en uno de lógica de negocio
    throw new ServiceException("Error al consultar los datos del usuario");
}

// Ejemplo 2: Relanzar la misma excepción (Inspección)
try {
    operacionCritica();
} catch (IOException e) {
    log.error("Fallo en la operación: " + e.getMessage());
    throw e; // Se relanza para que el llamador decida qué hacer
}
```


## 17. ¿En qué consiste que una excepción sea la **"causa"** de otra excepción? Pon un ejemplo en Java, donde capturemos una excepción de bajo nivel y la encapsulemos en otra personalizada de alto nivel. Cuando una excepción sale por pantalla y tiene una causa, ¿se ve?

### Respuesta

El concepto de "causa" u objetos excepción encadenados permite mantener el rastro original de un error cuando este es envuelto en una nueva excepción. En sistemas complejos, una excepción de bajo nivel (como un fallo de red o de base de datos) suele ser capturada y encapsulada en una excepción de nivel superior. Sin el mecanismo de causa, se perdería la información técnica original que explica por qué falló realmente la operación.

En Java, casi todos los constructores de las clases de excepción permiten pasar como argumento otro objeto Throwable que representa la causa raíz. Esto crea una jerarquía de errores donde el manejador final puede invocar el método getCause() para "pelar" las capas de la excepción y llegar al error inicial. Es una herramienta de depuración invaluable, ya que permite ver la historia completa del fallo sin romper la abstracción de las capas del software.

```
public void procesarPedido() {
    try {
        conectarServidorPago();
    } catch (SocketException e) {
        // Se envuelve la excepción técnica (causa) en una de negocio
        throw new PagoException("El servicio de pago no está disponible", e);
    }
}
```
Cuando una excepción encadenada se muestra por pantalla (por ejemplo, a través de printStackTrace()), sí se ve la causa. El sistema imprime primero la traza de la excepción más reciente y, justo debajo, añade una sección que comienza con el texto "Caused by:" seguido de la información y la traza de la excepción original. Esto permite al programador identificar rápidamente que, aunque el programa falló por una PagoException, el origen real fue una SocketException en una línea de código específica.