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

Un puntero a función en C es una variable que almacena la dirección de una función, de forma similar a cómo un puntero a dato almacena la dirección de una variable. Esto permite pasar funciones como parámetros, almacenarlas en estructuras o invocarlas indirectamente a través del puntero.

En el ejemplo, se define una función que recibe una cadena y devuelve una nueva cadena en mayúsculas. Después se declara un puntero a función con la firma adecuada y se invoca usando ese puntero.

```c
#include <stdio.h>
#include <ctype.h>
#include <string.h>

char* convertirAMayusculas(const char* texto, char* salida) {
    for (size_t i = 0; i < strlen(texto); i++) {
        salida[i] = toupper((unsigned char)texto[i]);
    }
    salida[strlen(texto)] = '\0';
    return salida;
}

int main(void) {
    char texto[] = "Hola Mundo";
    char resultado[100];

    char* (*aMayusculas)(const char*, char*) = convertirAMayusculas;
    printf("%s\n", aMayusculas(texto, resultado));

    return 0;
}
```

## 2. ¿Qué es una **función lambda** en un lenguaje de programación? Pon un ejemplo similar al anterior en Javascript y otro en Java con funciones lambda. Usa una variable local `aMayusculas` para apuntar a la función lambda. Por simplicidad, en Java, emplea `Function<String, String>` para el tipo de la referencia a la función lambda.

### Respuesta

Una función lambda es una función anónima, es decir, una función definida en el lugar donde se necesita sin un nombre propio. Se utiliza para expresar operaciones simples de forma compacta y, en lenguajes con soporte, puede asignarse a variables y pasarse como parámetro.

A continuación se muestra un ejemplo en JavaScript y en Java. En ambos casos, `aMayusculas` es una variable local que apunta a la función lambda y se invoca como si fuera cualquier otra función.

```javascript
const aMayusculas = texto => texto.toUpperCase();
console.log(aMayusculas('Hola Mundo'));
```

```java
import java.util.function.Function;

public class Main {
    public static void main(String[] args) {
        Function<String, String> aMayusculas = texto -> texto.toUpperCase();
        System.out.println(aMayusculas.apply("Hola Mundo"));
    }
}
```

## 3. ¿Qué es el **paradigma funcional**? ¿Por qué a algunos lenguajes orientados a objetos como Java 8, se les llama multi-paradigma? ¿Qué quiere decir que las funciones son "ciudadanos de primera clase"?

### Respuesta

El paradigma funcional es un estilo de programación que trata las funciones como valores, evita el estado mutable y favorece la composición y la transformación de datos. En este paradigma, se prefiere describir qué se quiere obtener en lugar de cómo cambiar el estado paso a paso.

Java 8 se considera multi-paradigma porque mantiene la programación orientada a objetos y añade características funcionales como lambdas, streams y referencias a métodos. Esto permite usar diferentes estilos según el problema y aprovechar lo mejor de cada enfoque.

Decir que las funciones son "ciudadanos de primera clase" significa que pueden ser creadas en tiempo de ejecución, asignadas a variables, pasadas como argumentos a otras funciones y devueltas como resultados. En otras palabras, las funciones se tratan como cualquier otro tipo de valor.

## 4. Explica la sintaxis básica de una función lambda en Java.

### Respuesta

La sintaxis básica de una lambda en Java consiste en los parámetros, la flecha `->` y el cuerpo de la función. Si hay un único parámetro, los paréntesis son opcionales; si hay varios, se usan paréntesis. El cuerpo puede ser una expresión simple o un bloque entre llaves.

Por ejemplo, `x -> x * 2` es una lambda que duplica un número. Para operaciones más complejas se usa el bloque `{ ... }` y se puede incluir `return` cuando sea necesario.

## 5. Ahora recibamos una función como parámetro a un método y la llamaremos desde dentro. Amplia los ejemplos anteriores de Java y JavaScript con un método llamado `transformar`, que reciba un `String` como parámetro y luego una función transformadora como lo es `aMayúsculas` y la invoque desde dentro.

### Respuesta

Recibir una función como parámetro implica declarar el tipo de esa función en la firma del método. En JavaScript basta pasar una función; en Java se emplea una interfaz funcional como `Function<String, String>`.

El método `transformar` toma el texto y la función transformadora, y devuelve el resultado de aplicar esa función. De esta forma se separa la lógica de transformación del lugar donde se usa.

```javascript
function transformar(texto, transformador) {
    return transformador(texto);
}

const aMayusculas = texto => texto.toUpperCase();
console.log(transformar('Hola Mundo', aMayusculas));
```

```java
import java.util.function.Function;

public class Main {
    public static String transformar(String texto, Function<String, String> transformador) {
        return transformador.apply(texto);
    }

    public static void main(String[] args) {
        Function<String, String> aMayusculas = texto -> texto.toUpperCase();
        System.out.println(transformar("Hola Mundo", aMayusculas));
    }
}
```

## 6. Ahora, invoca `transformar`, con una nueva función lambda directamente en la llamada a `transformar`, por ejemplo, una función lambda que invierta la cadena. Define la función de inversión justo cuando la estás pasando como parámetro.

### Respuesta

Se puede crear la función lambda en el mismo lugar donde se llama a `transformar`, sin necesidad de asignarla antes. Esta forma es útil para transformaciones puntuales y reduce la cantidad de variables locales.

El código muestra cómo pasar una lambda que invierte la cadena directamente a la llamada de `transformar`, usando el uso de `StringBuilder` o cualquier otra técnica de inversión.

```java
import java.util.function.Function;

public class Main {
    public static String transformar(String texto, Function<String, String> transformador) {
        return transformador.apply(texto);
    }

    public static void main(String[] args) {
        String invertido = transformar("Hola Mundo", texto -> new StringBuilder(texto).reverse().toString());
        System.out.println(invertido);
    }
}
```

## 7. ¿Qué se entiende por cierre o "closure" en el contexto de las funciones lambda? Pon un ejemplo en Java de cómo una función lambda es capaz de acceder a una variable local en el contexto donde fue definida. Modifica el ejemplo anterior, creando otra función lambda para transformar una cadena, pero que lo que haga es concatenar a la cadena de entrada otra cadena que está en una variable local definida fuera de la función lambda.

### Respuesta

Un closure es una función que captura variables del ámbito en el que fue creada y las utiliza más tarde cuando se ejecuta. En Java, una lambda puede acceder a variables locales del método que la define siempre que esas variables sean efectivamente finales o finales.

El ejemplo siguiente define una cadena `prefijo` en el método `main` y luego crea una lambda que concatena ese prefijo con el texto pasado. La lambda recuerda el valor de `prefijo` incluso cuando se invoca dentro de `transformar`.

```java
import java.util.function.Function;

public class Main {
    public static String transformar(String texto, Function<String, String> transformador) {
        return transformador.apply(texto);
    }

    public static void main(String[] args) {
        String prefijo = "Mensaje: ";
        Function<String, String> concatenaPrefijo = texto -> prefijo + texto;
        System.out.println(transformar("Hola Mundo", concatenaPrefijo));
    }
}
```

## 8. Reflexiona: ¿en qué se diferencia entonces una función lambda de los punteros a funciones que hay en C?

### Respuesta

Una función lambda es una función anónima con posible acceso a su contexto léxico, mientras que un puntero a función en C solo almacena la dirección de una función estática sin contexto. Eso significa que la lambda puede capturar variables del entorno y formar closures, mientras que el puntero en C no tiene información del estado externo.

Además, en Java la lambda tiene un tipo asociado basado en una interfaz funcional y su uso está controlado por el sistema de tipos. En C, el puntero a función solo verifica la firma y no hay soporte automático para capturar ambiente o encadenar comportamientos.

## 9. Devolvamos ahora funciones. Creemos ahora una función que sea capaz de crear funciones "descuento". Una función "descuento", decrementa un porcentaje pasado como parámetro. Por simplicidad, usa `Function<Double, Double>` para su tipo. La función `crearDescuento(porcentaje)`, recibe solo el porcentaje de descuento a aplicar y devuelve la función de descuento. Prueba a crear dos descuentos distintos y aplicarlos a una cantidad. Explica la closure en la función descuento.

### Respuesta

La función `crearDescuento` devuelve otra función que aplica un porcentaje fijo sobre un precio. El porcentaje se captura en el closure de la lambda, de modo que cada función devuelta recuerda su propio valor de descuento.

Al crear dos descuentos distintos y aplicarlos a la misma cantidad, se observa que cada función conserva el porcentaje que se pasó al crearla. El closure es la razón por la cual la lambda puede seguir usando `porcentaje` incluso después de que `crearDescuento` haya terminado.

```java
import java.util.function.Function;

public class Main {
    public static Function<Double, Double> crearDescuento(double porcentaje) {
        return precio -> precio * (1 - porcentaje / 100.0);
    }

    public static void main(String[] args) {
        Function<Double, Double> descuento10 = crearDescuento(10);
        Function<Double, Double> descuento25 = crearDescuento(25);

        double precio = 100.0;
        System.out.println(descuento10.apply(precio)); // 90.0
        System.out.println(descuento25.apply(precio)); // 75.0
    }
}
```

## 10. En Java, que es un lenguaje con comprobación estática de tipos, donde los tipos se declaran, toda función lambda tiene un tipo, que se conoce como **interfaz funcional**. ¿Qué es una **interfaz funcional**? ¿Qué requisitos tiene?

### Respuesta

Una interfaz funcional es una interfaz que declara exactamente un método abstracto. Esa única operación define la forma de la lambda, y el compilador usa el tipo de la interfaz para inferir cómo debe comportarse la expresión lambda.

Los requisitos son sencillos: debe tener un único método abstracto, aunque puede tener otros métodos `default` o `static`. También puede estar marcada con `@FunctionalInterface`, lo que ayuda a documentar la intención y evitar añadir accidentalmente más métodos abstractos.

## 11. Creemos una interfaz funcional a mano. Por ejemplo, define la interfaz funcional del ejemplo que transforma la cadena en otra. Llámale `Transformador`, que define una función que convierte una cadena de texto (`String`) en otra (`String`).

### Respuesta

La interfaz funcional `Transformador` define un solo método `transformar` que recibe una cadena y devuelve otra. Con esta interfaz se puede usar una lambda para convertir el texto de distintas formas.

```java
@FunctionalInterface
public interface Transformador {
    String transformar(String texto);
}

public class Main {
    public static String aplicarTransformacion(String texto, Transformador transformador) {
        return transformador.transformar(texto);
    }

    public static void main(String[] args) {
        Transformador aMayusculas = texto -> texto.toUpperCase();
        System.out.println(aplicarTransformacion("Hola Mundo", aMayusculas));
    }
}
```

## 12. Ahora hagamos la interfaz funcional algo más genérica y empleando generics, para que permita definir un `Transformador` de un tipo en otro. Pon un ejemplo de un transformador que redondea un `Double` en un `Integer`.

### Respuesta

La interfaz puede generalizarse usando parámetros de tipo `<T, R>`, donde `T` es el tipo de entrada y `R` el tipo de salida. Esto hace la interfaz reutilizable para muchas transformaciones diferentes.

El siguiente ejemplo muestra un `Transformador<Double, Integer>` que convierte un `Double` en su valor redondeado a `Integer`.

```java
@FunctionalInterface
public interface Transformador<T, R> {
    R transformar(T valor);
}

public class Main {
    public static <T, R> R aplicarTransformacion(T valor, Transformador<T, R> transformador) {
        return transformador.transformar(valor);
    }

    public static void main(String[] args) {
        Transformador<Double, Integer> redondear = valor -> (int) Math.round(valor);
        System.out.println(aplicarTransformacion(3.7, redondear));
    }
}
```

## 13. `Transformador`, en su versión genérica, parece muy útil y reutilizable, hasta el punto de que es igual a una interfaz funcional que ya hay, que es `Function<T, R>`. Muestra las interfaces funcionales predefinidas que hay en Java.

### Respuesta

Java ya incluye varias interfaces funcionales en el paquete `java.util.function`. Las más comunes son `Function<T, R>`, `Consumer<T>`, `Supplier<T>`, `Predicate<T>` y `BiFunction<T, U, R>`. Cada una representa un tipo de función diferente: con entrada y salida, solo entrada, solo salida, predicado booleano o dos entradas.

Estas interfaces predefinidas evitan definir nuevas interfaces para casos comunes y permiten interoperar con APIs estándar como streams y collections.

## 14. Vamos a ver ejemplos expresivos de funcional en Java. Estudiemos el `List.forEach`, como versión funcional del bucle `for`. Emplea el `forEach` para recorrer una lista de `Integer` y que muestre un mensaje si el entero es positivo.

### Respuesta

El método `forEach` recibe un `Consumer` y aplica esa acción a cada elemento de la colección. Es una forma más declarativa de recorrer la lista y separar la acción de la iteración.

```java
import java.util.Arrays;
import java.util.List;

public class Main {
    public static void main(String[] args) {
        List<Integer> numeros = Arrays.asList(5, -3, 0, 12);
        numeros.forEach(n -> {
            if (n > 0) {
                System.out.println(n + " es positivo");
            }
        });
    }
}
```

## 15. Repasando el tema de genericidad, fíjate en la firma de `forEach`, ¿por qué se usa `Consumer<? super T>` y no `Consumer<T>`? Explica qué significa **PECS**, y explícalo para el caso de mejorar el ejemplo del método `transformar` la hora de definir el tipo de la función transformadora.

### Respuesta

`forEach` usa `Consumer<? super T>` porque el consumidor puede aceptar el tipo `T` o cualquiera de sus supertipos. Esto permite usar consumidores más generales, como `Consumer<Object>`, sobre una lista de `Integer`, aumentando la flexibilidad sin romper la seguridad de tipos.

PECS significa "Producer Extends, Consumer Super". Para colecciones que producen valores se usa `? extends`, y para las que consumen valores se usa `? super`. En el ejemplo de `transformar`, si se definiera el tipo de transformador como `Function<? super String, ? extends String>`, se permitiría pasar funciones que acepten cualquier supertipo de `String` y devuelvan cualquier subtipo de `String`.

## 16. Referencias a métodos. Podemos obtener una referencia a métodos de objetos o clases. Pon un ejemplo en JavaScript y en Java, de una clase `Persona` con un método `saludar`. En el código principal, crea una `Persona` con un nombre, y obtén una referencia a su método `saludar` en una variable local. Invoca `saludar` con esa referencia a su método `saludar`.

### Respuesta

Una referencia a método funciona como un puntero a función en el contexto del objeto o clase. En JavaScript se puede extraer el método de la instancia y guardarlo en una variable, siempre que se preserve el contexto correcto.

En Java se usa la sintaxis `objeto::metodo` para obtener la referencia. La referencia puede invocarse como si fuera una función, habitualmente mediante una interfaz funcional.

```javascript
class Persona {
    constructor(nombre) {
        this.nombre = nombre;
    }

    saludar() {
        console.log(`Hola, soy ${this.nombre}`);
    }
}

const persona = new Persona('Ana');
const saludo = persona.saludar.bind(persona);
saludo();
```

```java
public class Persona {
    private final String nombre;

    public Persona(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Hola, soy " + nombre);
    }

    public static void main(String[] args) {
        Persona persona = new Persona("Ana");
        Runnable saludo = persona::saludar;
        saludo.run();
    }
}
```

## 17. ¿Qué tipos de referencias a método se pueden hacer en Java? Pon un ejemplo de referencia a método estático, a constructor, a método de instancia de una instancia concreta y a método de instancia sobre cualquier instancia.

### Respuesta

Java permite cuatro tipos principales de referencias a método: a método estático, a constructor, a método de instancia de una instancia concreta y a método de instancia sobre cualquier instancia de un tipo. Estas referencias se escriben con `::` y se ajustan a distintas interfaces funcionales.

El siguiente ejemplo muestra cada caso con breves expresiones.

```java
import java.util.function.Function;
import java.util.function.Supplier;
import java.util.function.BiFunction;

public class Persona {
    private final String nombre;

    public Persona(String nombre) {
        this.nombre = nombre;
    }

    public static void saludarEstatico() {
        System.out.println("Hola desde método estático");
    }

    public void saludar() {
        System.out.println("Hola, soy " + nombre);
    }

    public String obtenerNombre() {
        return nombre;
    }

    public static void main(String[] args) {
        Runnable refEstatico = Persona::saludarEstatico;
        Supplier<Persona> refConstructor = () -> new Persona("Carlos");
        Persona p = new Persona("Lucía");
        Runnable refInstancia = p::saludar;
        Function<Persona, String> refMetodoCualquierInstancia = Persona::obtenerNombre;

        refEstatico.run();
        Persona nueva = refConstructor.get();
        refInstancia.run();
        System.out.println(refMetodoCualquierInstancia.apply(nueva));
    }
}
```

## 18. Otro ejemplo expresivo. Ordena una lista de `Persona`, cada persona tiene un nombre y una edad (de tipo entero). Ordena la lista de `Persona` con `Collections.sort`, pasándole como comparador una expresión lambda que compare la edad de ambas personas y si tienen la misma edad, se ordene por orden alfabético del nombre. Crea dos versiones: Una con la función de comparación hecha manualmente, y otra empleando `Comparator`.

### Respuesta

La comparación manual define explícitamente el criterio dentro de la lambda, mientras que el uso de `Comparator` aprovecha métodos auxiliares para componer comparadores. Ambos enfoques permiten expresar la lógica de ordenación de forma más declarativa que un bucle tradicional.

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.Comparator;
import java.util.List;

public class Persona {
    private final String nombre;
    private final int edad;

    public Persona(String nombre, int edad) {
        this.nombre = nombre;
        this.edad = edad;
    }

    public String getNombre() {
        return nombre;
    }

    public int getEdad() {
        return edad;
    }

    public static void main(String[] args) {
        List<Persona> personas = new ArrayList<>();
        personas.add(new Persona("Ana", 30));
        personas.add(new Persona("Luis", 25));
        personas.add(new Persona("María", 30));

        Collections.sort(personas, (p1, p2) -> {
            int cmp = Integer.compare(p1.getEdad(), p2.getEdad());
            return cmp != 0 ? cmp : p1.getNombre().compareTo(p2.getNombre());
        });

        Collections.sort(personas, Comparator
            .comparingInt(Persona::getEdad)
            .thenComparing(Persona::getNombre));
    }
}
```