# TEMA 2. Encapsulación

## 1. En Programación Orientada a Objetos (POO), ¿Qué buscan la **encapsulación** y **la ocultación** de información? Enumera brevemente algunas ventajas de la ocultación de información.

### Respuesta

La encapsulación consiste en el empaquetamiento de datos y el comportamiento que los manipula dentro de una única unidad lógica llamada clase. A diferencia de C, donde los datos (struct) y las funciones suelen estar dispersos, la encapsulación busca que el objeto sea una entidad autónoma que gestione su propio estado.

La ocultación de información es el mecanismo mediante el cual se restringe el acceso a los detalles internos de un objeto. Mientras que la encapsulación agrupa los elementos, la ocultación decide qué partes son visibles para el exterior y cuáles son privadas. El objetivo principal es separar el "qué hace" un objeto de "cómo lo hace" internamente.

Entre las ventajas de la ocultación de información destacan la facilidad de mantenimiento, ya que se puede cambiar la implementación interna sin romper el código que usa el objeto, y la robustez, al evitar que agentes externos modifiquen variables internas de forma imprevista. También reduce la complejidad para el programador que utiliza la clase, al exponer solo lo estrictamente necesario.

## 2. ¿Qué se entiende por la **interfaz pública** de un objeto o clase en POO? Describe brevemente cómo se relaciona con la ocultación de información.

### Respuesta

La interfaz pública de una clase se define como el conjunto de métodos, constructores y constantes que son accesibles por otras clases. Se puede visualizar como el "contrato" o el manual de instrucciones que el objeto ofrece al resto del sistema para que este pueda interactuar con él de manera segura.

La relación con la ocultación de información es de complementariedad. La ocultación protege el estado interno (los datos sensibles), mientras que la interfaz pública actúa como el único canal de comunicación permitido. Es la capa visible que oculta la complejidad subyacente del objeto.

Si se establece una interfaz pública minimalista y bien definida, se garantiza que el usuario del objeto no dependa de cómo están almacenados los datos. Esto permite que el objeto evolucione internamente sin que los programas que lo utilizan tengan que ser modificados.

## 3. Brevemente: ¿Por qué hay que ser conscientes y diseñar con cuidado la **interfaz pública** de una clase? ¿Es fácil cambiarla?

### Respuesta

El diseño de la interfaz pública requiere un cuidado extremo porque representa el compromiso de la clase con el mundo exterior. Una vez que otros módulos o programadores empiezan a utilizar los métodos públicos de una clase, cualquier cambio en sus nombres, parámetros o tipos de retorno obligará a reescribir y recompilar todo el código dependiente.

Por lo tanto, no es fácil cambiar una interfaz pública una vez que se ha publicado o integrado en un proyecto de gran envergadura. Mientras que la parte privada (oculta) puede reescribirse por completo sin que nadie lo note, una modificación en la firma de un método público suele generar errores en cadena en el sistema.

Un buen diseño debe buscar la estabilidad, exponiendo únicamente lo necesario para cumplir con la funcionalidad requerida. Esto se conoce como el principio de mínimo privilegio aplicado al diseño de software, donde se minimiza la superficie de contacto para reducir el acoplamiento entre clases.

## 4. ¿Qué son las **invariantes de clase** y por qué la ocultación de información nos ayuda?

### Respuesta

Las invariantes de clase son condiciones o reglas lógicas que deben cumplirse siempre para que un objeto se considere que está en un estado válido. Por ejemplo, en una clase que gestione una fecha, una invariante sería que el valor del "día" siempre debe ser coherente con el "mes" y el "año" (no permitir un 31 de abril).

La ocultación de información es la herramienta fundamental para proteger estas invariantes. Al marcar los atributos como privados (private), se impide que un programador externo asigne valores arbitrarios que rompan la lógica del objeto, como ocurriría si se tuviera acceso directo a una variable en una estructura de C.

Al centralizar el acceso a los datos a través de métodos públicos controlados, la propia clase puede validar la información antes de realizar cualquier cambio. Si los datos no cumplen con las reglas establecidas, el objeto puede rechazar la operación, garantizando así que nunca entre en un estado corrupto o inconsistente.

## 5. Pon un ejemplo de una clase `Punto` en `Java`, con dos coordenadas, `x` e `y`, de tipo `double`, con un método `calcularDistanciaAOrigen`, y que haga uso de la ocultación de información. ¿Cuál es la interfaz pública de la clase `Punto`? ¿Qué significa `public` y `private`?

### Respuesta
```
public class Punto {
    // Atributos ocultos al exterior
    private double x;
    private double y;

    // Constructor público
    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    // Método de la interfaz pública
    public double calcularDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }
}
```
La interfaz pública de esta clase Punto está constituida por el constructor Punto(double x, double y) y el método calcularDistanciaAOrigen(). Los atributos x e y no forman parte de ella, ya que se han protegido deliberadamente para que no puedan ser alterados directamente desde fuera de la clase.

En este contexto, el modificador private indica que el miembro (dato o función) solo es accesible por el código que pertenece a la propia clase. Por el contrario, el modificador public permite que cualquier otra parte del programa, independientemente del paquete o clase en la que se encuentre, pueda invocar ese método o constructor.

Para un programador de C/C++, esto equivale a la diferencia entre variables locales de un archivo que no se exportan en el encabezado (.h) y funciones cuya firma sí se publica para que otros archivos de código fuente puedan enlazarlas.


## 6. En Java, ¿A quiénes se pueden aplicar los modificadores `public` o `private`?

### Respuesta

En Java, los modificadores de acceso public y private se pueden aplicar principalmente a los miembros de una clase, lo que incluye variables (atributos), métodos y constructores. Al marcar un miembro como private, se restringe su visibilidad únicamente al ámbito de la propia clase, mientras que public permite el acceso desde cualquier otra parte del programa.

También se pueden aplicar a las clases de nivel superior, aunque con ciertas restricciones. Una clase puede ser declarada como public, lo que permite que sea utilizada por cualquier otra clase en cualquier paquete. Sin embargo, una clase de nivel superior no puede ser declarada como private, ya que no tendría sentido una clase que no puede ser instanciada ni accedida por ninguna otra; este nivel de restricción solo se permite en clases internas o anidadas.

Para un programador con base en C, esto es conceptualmente similar a decidir qué funciones o variables globales se declaran en el archivo de cabecera (.h) para que sean visibles por otros módulos, y cuáles se mantienen como static dentro del archivo de implementación (.c) para que sean locales a esa unidad de traducción.


## 7. En POO, la visibilidad puede ser pública o privada, pero ¿existen más tipos de visibilidad? ¿Qué ocurre en Java? ¿Y en otros lenguajes?

### Respuesta

Además de los niveles extremos de público y privado, la Programación Orientada a Objetos suele ofrecer matices intermedios para facilitar la colaboración entre clases relacionadas. En Java, existen cuatro niveles de visibilidad: public, private, protected y la visibilidad por defecto (denominada package-private). El nivel protected permite el acceso a la propia clase, a sus subclases y a cualquier otra clase que se encuentre en el mismo paquete, mientras que el nivel por defecto (cuando no se escribe modificador) limita el acceso solo a las clases del mismo paquete.

Otros lenguajes implementan esquemas similares pero con variaciones. En C++, los niveles son básicamente public, private y protected, pero existe la palabra clave friend, que permite otorgar permisos de acceso a miembros privados a funciones o clases específicas, rompiendo de forma controlada la encapsulación. Por su parte, lenguajes como Python no tienen modificadores de acceso estrictos forzados por el compilador y se basan en convenciones de nomenclatura (como el prefijo de guion bajo _ o doble guion bajo __) para indicar que un miembro es para uso interno.

Estos niveles intermedios son cruciales en el desarrollo de librerías y frameworks, ya que permiten que ciertos componentes del sistema se comuniquen íntimamente entre sí sin exponer esos detalles internos al usuario final de la API.


## 8. Responde: Los miembros de instancia privados de un objeto están ocultos para (a) otras clases o (b) otras instancias, aunque sean de la misma clase. Pon un ejemplo añadiendo un método `calcularDistanciaAPunto(Punto otro)` y explica la respuesta.

### Respuesta

La respuesta correcta es la (a): están ocultos para otras clases. En Java (al igual que en C++), la visibilidad privada se define a nivel de clase y no a nivel de instancia. Esto significa que un objeto de la clase Punto puede acceder a los atributos privados de cualquier otro objeto de la clase Punto.

```
public double calcularDistanciaAPunto(Punto otro) {
    // Es posible acceder a otro.x y otro.y directamente aunque sean privados
    // porque este código reside dentro de la clase Punto.
    double dx = this.x - otro.x;
    double dy = this.y - otro.y;
    return Math.sqrt(dx * dx + dy * dy);
}
```
Esta característica es fundamental para implementar métodos que operan con varios objetos del mismo tipo, como comparadores de igualdad o cálculos geométricos. Si la visibilidad fuera por instancia (opción b), el código anterior daría un error de compilación y obligaría al uso de métodos públicos intermedios, lo que resultaría menos eficiente y más verboso para operaciones internas de la propia clase.


## 9. ¿Qué son los métodos "getter" y "setter" en los lenguajes orientados a objetos?

### Respuesta

Los métodos "getter" (captadores) y "setter" (modificadores) son métodos públicos diseñados específicamente para leer o actualizar el valor de un atributo privado, respectivamente. En la terminología formal de la POO, a menudo se les denomina métodos de acceso y métodos de mutación. Su función principal es actuar como intermediarios controlados entre el estado interno del objeto y el mundo exterior.

Un "getter" simplemente devuelve el valor del atributo, mientras que un "setter" recibe un parámetro y lo asigna a la variable interna. La gran ventaja de este enfoque frente a hacer el atributo público es la capacidad de añadir lógica de validación. Por ejemplo, un setter para un atributo edad puede incluir una condición para asegurar que el valor nunca sea negativo antes de realizar la asignación.

Además de la validación, estos métodos permiten que la representación interna del dato cambie sin afectar a quienes lo usan. Un objeto podría almacenar una temperatura en grados Celsius internamente, pero ofrecer getters y setters que operen en Fahrenheit, realizando la conversión de manera transparente para el usuario de la clase.


## 10. Cuando nos referimos a que la ocultación de información mejora la "seguridad" del programa, ¿nos referimos a que no pueda ser "hackeado"?

### Respuesta

No, en el contexto del diseño de software y la POO, el término "seguridad" no se refiere a la ciberseguridad o protección contra ataques externos (hackeo), sino a la integridad y robustez lógica del código. Se trata de evitar que el programa entre en estados inconsistentes o produzca errores debido a manipulaciones accidentales de los datos.

La ocultación de información protege al desarrollador de cometer errores al impedir que se modifiquen variables internas que dependen de otras o que deben seguir reglas estrictas (invariantes). Al restringir el acceso, se garantiza que los cambios en el estado del objeto solo ocurran a través de los métodos autorizados que contienen la lógica de control necesaria.

En definitiva, mejora la seguridad en el sentido de que el comportamiento del sistema se vuelve más predecible y fácil de depurar. Un error en el valor de un atributo privado es mucho más sencillo de localizar, ya que solo hay un número limitado de métodos dentro de la propia clase que han podido modificarlo, a diferencia de un atributo público que podría haber sido alterado desde cualquier parte del proyecto.


## 11. ¿Qué diferencia hay entre **miembro de instancia** y **miembro de clase**? ¿Los miembros de clase también se pueden ocultar?

### Respuesta

Un miembro de instancia es aquel que pertenece a cada objeto individualmente. En términos de C, es equivalente a un campo dentro de una struct: cada vez que se declara una variable de ese tipo, se reserva memoria para esos datos específicos. Si se crean diez objetos de una clase, existirán diez copias distintas de sus atributos de instancia.

Por el contrario, un miembro de clase se define mediante la palabra clave static y pertenece a la clase en su totalidad, no a un objeto concreto. Solo existe una única copia de este miembro en la memoria durante toda la ejecución del programa, independientemente de cuántos objetos se instancien. Es un concepto análogo a una variable global en C, pero con el beneficio de estar encapsulada dentro del espacio de nombres (el scope) de la clase.

Los miembros de clase también se pueden ocultar utilizando el modificador private. Al hacerlo, la variable o el método estático solo podrá ser manipulado por los métodos de su propia clase. Esto es extremadamente útil para gestionar configuraciones compartidas o contadores internos que no deben ser alterados caprichosamente desde el exterior, manteniendo así la integridad del estado global de la clase.


## 12. Brevemente: ¿Tiene sentido que los constructores sean privados?

### Respuesta

Aunque pueda parecer contradictorio, declarar un constructor como private es una práctica común y muy útil en el diseño orientado a objetos. Su función principal es impedir que otras clases puedan crear instancias de esa clase de forma libre mediante el operador new. Esto otorga a la clase el control total sobre su propio proceso de instanciación.

Esta técnica es la base para implementar el patrón de diseño Singleton, donde se garantiza que solo exista una única instancia de una clase en toda la aplicación (por ejemplo, para un gestor de base de datos o un sistema de log). También es habitual en clases de utilidad que solo contienen métodos estáticos (como la clase Math de Java), donde no tiene sentido crear objetos de ese tipo.

Asimismo, los constructores privados permiten el uso de métodos factoría. En este escenario, la clase ofrece un método público estático que se encarga de llamar al constructor privado tras realizar validaciones o configuraciones previas, devolviendo el objeto ya listo para su uso. Esto evita que el usuario de la clase cree objetos en estados inconsistentes o con parámetros inválidos.


## 13. ¿Cómo se indican los **miembros de clase** en Java? Pon un ejemplo, en la clase `Punto` definida anteriormente, para que incluya miembros de clase que permitan saber cuáles son los valores `x` e `y` máximos que se han establecido en todos los puntos que se hayan creado hasta el momento.

### Respuesta

En Java, los miembros de clase se indican anteponiendo la palabra reservada static a la declaración del atributo o método. Esto informa al compilador de que ese miembro debe ser compartido por todas las instancias de la clase y que su ciclo de vida está ligado a la clase misma, no a la creación de objetos individuales.

```
public class Punto {
    private double x;
    private double y;
    
    // Miembros de clase para rastrear máximos globales
    private static double maxX = Double.NEGATIVE_INFINITY;
    private static double maxY = Double.NEGATIVE_INFINITY;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
        
        // Actualización de los miembros de clase compartidos
        if (x > maxX) maxX = x;
        if (y > maxY) maxY = y;
    }

    public static double getMaxX() { return maxX; }
    public static double getMaxY() { return maxY; }
}
```
Al utilizar static, se garantiza que cada vez que se cree un nuevo Punto, los valores de maxX y maxY se actualicen en la única zona de memoria compartida. De este modo, cualquier parte del programa puede consultar el valor máximo alcanzado llamando a Punto.getMaxX(), sin necesidad de conocer qué objeto específico provocó ese máximo.


## 14. Como sería un método factoría dentro de la clase `Punto` para construir un `Punto` a partir de dos coordenadas, pero que las redondee al entero más cercano. Escribe sólo el código del método, no toda la clase ¿Has usado `static`? 

### Respuesta

```
public static Punto crearPuntoRedondeado(double x, double y) {
    double xRedondeado = Math.round(x);
    double yRedondeado = Math.round(y);
    return new Punto(xRedondeado, yRedondeado);
}
```
Efectivamente, se ha utilizado el modificador static. Esto es obligatorio porque un método factoría debe poder invocarse sin que exista previamente una instancia de la clase. Al ser estático, el método se asocia a la clase Punto y no a un objeto concreto, permitiendo que el usuario lo llame directamente (por ejemplo: Punto p = Punto.crearPuntoRedondeado(2.8, 4.1);).

Desde la perspectiva de alguien que viene de C, un método factoría estático funciona de manera muy similar a una función global que devuelve una estructura inicializada, pero con la ventaja de que reside dentro del ámbito de la clase, lo que mejora la organización del código y respeta el principio de encapsulación al centralizar la lógica de creación.


## 15. Cambia la implementación de `Punto`. En vez de dos `double`, emplea un array interno de dos posiciones, intentando no modificar la interfaz pública de la clase.

### Respuesta

```
public class Punto {
    // La representación interna cambia a un array
    private double[] coordenadas = new double[2];

    public Punto(double x, double y) {
        this.coordenadas[0] = x;
        this.coordenadas[1] = y;
    }

    // Se mantiene la interfaz pública original
    public double calcularDistanciaAOrigen() {
        return Math.sqrt(Math.pow(this.coordenadas[0], 2) + Math.pow(this.coordenadas[1], 2));
    }
    
    // Los métodos de acceso (getters) si existieran, seguirían devolviendo doubles individuales
    public double getX() { return this.coordenadas[0]; }
    public double getY() { return this.coordenadas[1]; }
}
```
Este ejercicio demuestra la potencia de la ocultación de información. Aunque se ha modificado completamente la estructura de almacenamiento interna (pasando de variables escalares a un array de dos posiciones), cualquier programa que utilizara la clase Punto anterior no sufrirá ningún cambio ni requerirá ser reescrito.

Para el usuario de la clase, el objeto sigue comportándose exactamente igual, ya que la firma de los métodos públicos (su nombre, parámetros y tipo de retorno) permanece idéntica. Esta capacidad de alterar el "cómo" sin afectar al "qué" es lo que permite que los sistemas complejos evolucionen y se mantengan de forma eficiente, evitando la fragilidad característica del código donde los datos son accesibles globalmente.


## 16. Si un atributo va a tener un método "getter" y "setter" públicos, ¿no es mejor declararlo público? ¿Cuál es la convención más habitual sobre los atributos, que sean públicos o privados? ¿Tiene esto algo que ver con las "invariantes de clase"?

### Respuesta

Aunque parezca redundante tener métodos que simplemente leen o escriben una variable, nunca es mejor declararlo público. Para un programador de C, una struct con campos públicos es lo normal, pero en POO esto rompe el principio de protección. Si el atributo es público, la clase pierde el control total sobre él: cualquier código externo puede cambiar su valor sin que el objeto pueda validarlo, auditarlo o reaccionar al cambio.

La convención universal en Java es que todos los atributos deben ser privados. Los métodos "getter" y "setter" actúan como una capa de abstracción. Esta capa permite que, en el futuro, se pueda cambiar la forma en que se almacena el dato (por ejemplo, calcularlo al vuelo en lugar de guardarlo) sin que el código que usa la clase tenga que ser modificado. Si el atributo fuera público, cualquier cambio interno obligaría a refactorizar todo el proyecto.


## 17. ¿Qué significa que una clase sea **inmutable**? ¿qué es un método modificador? ¿Un método modificador es siempre un "setter"? ¿Tiene ventajas que una clase sea inmutable?

### Respuesta

Una clase es inmutable cuando su estado no puede cambiar una vez que el objeto ha sido construido. Esto significa que, tras la ejecución del constructor, los valores de sus atributos permanecen constantes durante toda la vida del objeto. En Java, esto se logra declarando los atributos como private y final, y no proporcionando ningún método que permita alterar dichos valores.

Un método modificador es cualquier método que altera el estado interno del objeto. Aunque los "setters" son los modificadores más obvios, no son los únicos; cualquier método que realice una operación que cambie un valor interno (como un método acumular() o resetear()) es un modificador. En una clase estrictamente inmutable, estos métodos no existen o, en su lugar, devuelven un nuevo objeto con el dato modificado sin tocar el original.

Las ventajas de la inmutabilidad son significativas. Los objetos inmutables son intrínsecamente seguros para hilos (thread-safe), ya que no hay riesgo de que un hilo modifique el dato mientras otro lo lee. Además, simplifican enormemente la depuración y el razonamiento lógico, ya que se tiene la certeza de que el objeto no cambiará por "efectos secundarios" en otras partes del programa. Son ideales para representar valores constantes como colores, puntos geográficos o fechas.


## 18. ¿Es recomendable incluir métodos "setter" siempre y como convención?

### Respuesta

Rotundamente no. Existe la mala práctica de generar automáticamente getters y setters para cada atributo por inercia, lo que a menudo se denomina "anemia del modelo de dominio". Cada setter que se añade es una vulnerabilidad potencial en la encapsulación del objeto, ya que permite que el mundo exterior dicte cómo debe cambiar el estado interno.

El diseño debe guiarse por la necesidad real del negocio. Si un dato no necesita cambiar después de la creación del objeto (como el DNI de una persona o la fecha de creación de un registro), no debe tener un setter. Limitar la mutabilidad hace que el código sea más robusto y menos propenso a errores difíciles de rastrear producidos por cambios de estado inesperados.

Antes de incluir un setter, es preferible considerar si el cambio de estado debería ocurrir a través de un método con un nombre más descriptivo que valide reglas de negocio complejas. En lugar de un simple setSaldo(double s), es más seguro y encapsulado tener métodos como ingresar(double cantidad) o retirar(double cantidad), que protegen las invariantes de la clase.


## 19. ¿La clase `String` en Java es mutable o inmutable? ¿Qué ocurre al concatenar dos cadenas? ¿Qué debemos hacer si vamos a hacer una operación que implique concatenar muchas veces para construir paso a paso una cadena muy larga?

### Respuesta

La clase String en Java es inmutable. Una vez que se crea un objeto de texto, su contenido no se puede modificar en la memoria. Esta decisión de diseño permite que Java optimice el uso de memoria mediante el "String Pool" y garantiza que las cadenas sean seguras para ser compartidas como claves en mapas o entre diferentes hilos de ejecución sin riesgos.

Cuando se utiliza el operador + para concatenar dos cadenas (por ejemplo, s1 + s2), Java no modifica la cadena original. Lo que ocurre es que se reserva espacio para un tercer objeto String completamente nuevo que contiene la unión de ambos. Si realizamos esta operación dentro de un bucle miles de veces, el programa generará una enorme cantidad de objetos intermedios basura, lo que degradará drásticamente el rendimiento y saturará el recolector de basura (Garbage Collector).

Para construir cadenas largas de forma incremental, se debe utilizar la clase StringBuilder (o StringBuffer si se requiere sincronización entre hilos). StringBuilder funciona de manera similar a un array de caracteres dinámico en C que puede crecer; permite modificar su contenido interno sin crear nuevos objetos en cada paso. Solo al final, cuando la cadena está completa, se invoca el método .toString() para obtener el objeto inmutable definitivo.


## 20. En POO ¿Cómo se comparan objetos de una misma clase? ¿Por su contenido o por su identidad? ¿Qué es el método equals en Java? ¿Qué hace por defecto? ¿Cómo se deben comparar dos cadenas en Java? 

### Respuesta

En Java, existen dos formas de comparar objetos: por identidad y por contenido. La identidad se comprueba con el operador == y determina si dos variables apuntan exactamente a la misma dirección de memoria (el mismo objeto). El contenido se refiere a si dos objetos distintos tienen los mismos valores en sus atributos, lo cual es conceptualmente equivalente a comparar dos estructuras en C campo a campo.

El método equals(Object obj) es la herramienta estándar en Java para la comparación por contenido. Sin embargo, por defecto (tal como viene heredado de la clase base Object), el método equals hace exactamente lo mismo que el operador ==: comparar referencias de memoria. Para que una clase propia permita comparaciones de contenido reales, el programador debe sobrescribir (override) el método equals y definir qué campos determinan la igualdad.

En el caso de las cadenas de texto, siempre se deben comparar con el método .equals(). Debido a la inmutabilidad y al String Pool, a veces == puede dar true para dos cadenas con el mismo texto por una optimización interna, pero esto no está garantizado. Usar == para comparar el contenido de dos cadenas es uno de los errores más comunes y peligrosos para quienes vienen de otros lenguajes; la regla de oro es: identidad con ==, contenido con .equals().


## 21. ¿Qué son las clases "wrapper" en un lenguaje de programación orientado a objetos? ¿Cómo se hace? ¿Es un proceso automático? ¿Qué ventajas tienen? ¿Todos los lenguajes orientados a objetos tienen tipos primitivos y necesitan wrappers? 

### Respuesta

Las clases "wrapper" (envoltorios) son clases diseñadas para representar tipos de datos primitivos (como int, double o char) como si fueran objetos reales. En Java, cada tipo primitivo tiene su contraparte: Integer para int, Double para double, etc. Esto es necesario porque en Java existen estructuras, como las colecciones (ArrayList, HashMap), que solo pueden almacenar referencias a objetos y no valores de tipos primitivos directos.

En versiones modernas de Java, el proceso de convertir un primitivo a su objeto envoltorio y viceversa es automático y se conoce como autoboxing y unboxing. El compilador se encarga de realizar la conversión de manera transparente para el programador, lo que permite escribir código más limpio sin tener que instanciar manualmente las clases envoltorio.

La ventaja principal de estas clases es que permiten dotar a los tipos básicos de métodos de utilidad (como Integer.parseInt() para convertir texto a número) y permiten que estos participen en el ecosistema de tipos genéricos de Java. No obstante, no todos los lenguajes necesitan este concepto. Lenguajes considerados "puramente orientados a objetos", como Ruby o Smalltalk, no tienen tipos primitivos; en ellos, incluso un simple número entero es ya un objeto con sus propios métodos, eliminando la necesidad de envoltorios.


## 22. ¿En POO qué es un **tipo de dato enumerado**? ¿En Java, un tipo de dato enumerado es una clase? ¿Qué ventajas tienen en términos de encapsulación los enumerados en Java?

### Respuesta

Un tipo de dato enumerado (enum) es una estructura que permite definir un conjunto fijo de constantes con nombre, representando una lista cerrada de opciones (como los días de la semana o los estados de un pedido). A diferencia de C, donde un enum es esencialmente un alias para una lista de números enteros, en Java un enumerado es una clase especializada.

Al ser una clase, los enumerados en Java pueden tener sus propios atributos, constructores y métodos. Cada constante del enumerado se comporta como una instancia única y predefinida de dicha clase. Esto permite que el enumerado no sea solo una etiqueta, sino un objeto inteligente capaz de almacenar datos adicionales y realizar operaciones lógicas relacionadas con el valor que representa.

En términos de encapsulación, los enumerados de Java ofrecen una ventaja crítica: permiten agrupar toda la lógica y la información relacionada con una constante dentro de la misma definición del tipo. Esto evita tener datos dispersos por el código (como arrays de nombres o de valores asociados) y garantiza la seguridad de tipos, ya que el compilador impide que se asigne cualquier valor que no sea uno de los definidos explícitamente en el enum.



## 23. Crea un tipo enumerado en Java que se llame `Mes`, con doce posibles instancias y que además proporcione métodos para obtener cuántos días tiene ese mes, el ordinal de ese mes en el año (1-12), empleando atributos privados y constructores del tipo enumerado.

### Respuesta

```
public enum Mes {
    ENERO(31, 1), FEBRERO(28, 2), MARZO(31, 3), ABRIL(30, 4),
    MAYO(31, 5), JUNIO(30, 6), JULIO(31, 7), AGOSTO(31, 8),
    SEPTIEMBRE(30, 9), OCTUBRE(31, 10), NOVIEMBRE(30, 11), DICIEMBRE(31, 12);

    // Atributos privados para encapsular la información del mes
    private final int dias;
    private final int ordinal;

    // El constructor de un enum siempre es privado implícitamente
    Mes(int dias, int ordinal) {
        this.dias = dias;
        this.ordinal = ordinal;
    }

    public int getDias() {
        return this.dias;
    }

    public int getOrdinal() {
        return this.ordinal;
    }
}
```

En este diseño, la información de cada mes queda perfectamente protegida bajo el principio de ocultación de información. Los datos (dias y ordinal) se definen como final para garantizar la inmutabilidad de las constantes. Para un programador de C, esta estructura es mucho más potente que un simple enum y un conjunto de funciones externas, ya que el objeto Mes "sabe" sus propias propiedades.


## 24. Añade a la clase `Mes` del ejercicio anterior cuatro métodos para devolver si ese mes tiene algunos días de invierno, primavera, verano u otoño, indicando con un booleano el hemisferio (norte o sur, parámetro `enHemisferioNorte`). Es decir: `esDePrimavera(boolean esHemisferioNorte)`, `esDeVerano(boolean esHemisferioNorte)`, `esDeOtoño(boolean esHemisferioNorte)`, `esDeInvierno(boolean esHemisferioNorte)`

### Respuesta

```
public boolean esDePrimavera(boolean enHemisferioNorte) {
    if (enHemisferioNorte) {
        return (this == MARZO || this == ABRIL || this == MAYO);
    }
    return (this == SEPTIEMBRE || this == OCTUBRE || this == NOVIEMBRE);
}

public boolean esDeVerano(boolean enHemisferioNorte) {
    if (enHemisferioNorte) {
        return (this == JUNIO || this == JULIO || this == AGOSTO);
    }
    return (this == DICIEMBRE || this == ENERO || this == FEBRERO);
}

public boolean esDeOtoño(boolean enHemisferioNorte) {
    if (enHemisferioNorte) {
        return (this == SEPTIEMBRE || this == OCTUBRE || this == NOVIEMBRE);
    }
    return (this == MARZO || this == ABRIL || this == MAYO);
}

public boolean esDeInvierno(boolean enHemisferioNorte) {
    if (enHemisferioNorte) {
        return (this == DICIEMBRE || this == ENERO || this == FEBRERO);
    }
    return (this == JUNIO || this == JULIO || this == AGOSTO);
}
```

Este enfoque demuestra cómo la POO permite encapsular reglas de negocio complejas dentro del tipo de dato. En lugar de tener un switch gigante en el programa principal para determinar la estación, se le pregunta directamente al objeto Mes. Esto hace que el código sea mucho más legible, fácil de testear y resistente a errores, ya que la lógica estacional reside exclusivamente donde pertenece: en la definición del mes.