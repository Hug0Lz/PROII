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

### Respuesta

Usando `Object` en Java es posible crear estructuras de datos que almacenen cualquier tipo de dato, ya que todas las clases heredan de `Object`. Con un array primitivo de `Object` se puede implementar una lista dinámica que crezca cuando sea necesario y que acepte `String`, `Integer`, `Double` u otros objetos personales.

```java
public class ListaUniversal {
    private Object[] datos;
    private int tamaño;

    public ListaUniversal() {
        datos = new Object[10];
        tamaño = 0;
    }

    public void agregar(Object valor) {
        if (tamaño == datos.length) {
            Object[] nuevo = new Object[datos.length * 2];
            System.arraycopy(datos, 0, nuevo, 0, datos.length);
            datos = nuevo;
        }
        datos[tamaño++] = valor;
    }

    public Object obtener(int indice) {
        if (indice < 0 || indice >= tamaño) {
            throw new IndexOutOfBoundsException("Índice fuera de rango");
        }
        return datos[indice];
    }
}
```

Este ejemplo es equivalente a usar `void*` en C, porque almacena referencias heterogéneas, pero no ofrece comprobación de tipos en tiempo de compilación. Para recuperar valores concretos es necesario convertirlos explícitamente al tipo esperado.

## 2. Brevemente, ¿Qué significa la **programación genérica**? ¿Es el ejemplo anterior un ejemplo básico de programación genérica? 

### Respuesta

La programación genérica es la técnica de escribir código que funciona con múltiples tipos de datos sin duplicación y con comprobación de tipos en tiempo de compilación. En lugar de codificar una clase para cada tipo concreto, se usa un parámetro de tipo que se sustituye por tipos reales al instanciar la clase o invocar el método.

El ejemplo anterior con `Object` no es un buen ejemplo de programación genérica moderna: es una forma primitiva que permite almacenar cualquier cosa pero sacrifica la seguridad de tipos y requiere castings manuales. La verdadera programación genérica usa parámetros de tipo para conservar la información del tipo concreto y evitar errores en tiempo de ejecución.

## 3. Indica los problemas respecto al chequeo de tipos, de emplear `void*` o `Object` cuando se crean estructuras de datos genéricas. 

### Respuesta

El problema principal es que el compilador ya no puede garantizar que el tipo de dato insertado sea el mismo que el extraído. Esto provoca la necesidad de castings explícitos cuando se recuperan elementos, y esos castings pueden fallar en tiempo de ejecución si el tipo real no coincide con el esperado.

Además, el uso de `void*` en C o `Object` en Java hace que no se detecten errores de tipo en compilación. Si se inserta un `Double` en una lista que debería contener `String`, el programa compilará pero puede lanzar un `ClassCastException` en Java o provocar un comportamiento indefinido en C cuando se lea el dato con un tipo incorrecto.

## 4. Vamos entonces con mecanismos de mejora de la programación genérica ¿Qué son los **parámetros de tipo**? 

### Respuesta

Los parámetros de tipo son marcadores que permiten definir una clase o un método de forma genérica, usando un nombre simbólico como `T`, `E`, `K` o `V` en lugar de un tipo concreto. Al instanciar la clase o llamar al método, se sustituye ese parámetro por un tipo real, y el compilador puede verificar que el uso es consistente.

Por ejemplo, en `List<T>`, `T` representa el tipo de elemento de la lista. Si se crea `List<String>`, `T` se interpreta como `String` en ese contexto. Esto permite que el compilador compruebe que solo se insertan `String` en esa lista y que los métodos devuelven `String` sin necesidad de castings.

## 5. En Java existe "generics", en C++ existen "templates". Pon un ejemplo de uso de programación genérica en ambos, instanciando una lista o vector dinámico que solo admite `String`. Introduce valores, y luego haz un recorrido de ellos mostrando cómo cada elemento es del tipo concreto con seguridad.

### Respuesta

En Java, los generics permiten crear colecciones tipadas que evitan castings. Al declarar `List<String>`, el compilador impone que solo se inserten `String` y permite trabajar con cada elemento como `String` de forma segura.

```java
import java.util.ArrayList;
import java.util.List;

public class Main {
    public static void main(String[] args) {
        List<String> lista = new ArrayList<>();
        lista.add("Hola");
        lista.add("Mundo");
        lista.add("Generics");

        for (String elemento : lista) {
            System.out.println(elemento.toUpperCase());
        }
    }
}
```

En C++, los templates cumplen una función similar generando código específico para cada tipo. Un `vector<string>` solo admite `string` y permite iterar con seguridad usando el tipo concreto.

```cpp
#include <iostream>
#include <string>
#include <vector>

int main() {
    std::vector<std::string> lista;
    lista.push_back("Hola");
    lista.push_back("Mundo");
    lista.push_back("Templates");

    for (const std::string& elemento : lista) {
        std::cout << elemento << std::endl;
    }
    return 0;
}
```

En ambos ejemplos, el compilador sabe que los elementos son `String` o `string`, evitando castings y errores de tipo.

## 6. Sobre el funcionamiento de la programación genérica. ¿Qué hace el compilador cuando se instancia una clase que tiene parámetros de tipo? ¿Hace lo mismo C++ y Java? ¿Qué es el "type erasure" de Java y la "instanciación de plantillas" de C++?

### Respuesta

Java y C++ manejan los parámetros de tipo de forma distinta. En Java, el compilador aplica "type erasure": sustituye los parámetros de tipo por sus límites (por defecto `Object` o el límite superior) y genera código bytecode sin información de tipos genéricos. Esto significa que en tiempo de ejecución no existe información sobre `List<String>` frente a `List<Integer>`.

En C++, el compilador realiza instanciación de plantillas: genera una versión distinta de la clase o función para cada tipo concreto. Es decir, `vector<int>` y `vector<string>` se compilan como código separado, conservando la información de tipo en tiempo de ejecución y permitiendo optimizaciones específicas. El enfoque de Java prioriza compatibilidad binaria y menor tamaño de bytecode, mientras que C++ prioriza eficiencia y tipado estático completo.

## 7. Vamos a crear una nueva clase con parámetros de tipo. Define en Java una clase `Par`, que permite alojar dos valores de tipos diferentes. Incluye un constructor y un getter para cada tipo. Pon un ejemplo de uso de ese `Par`, por ejemplo para especificar el tipo de retorno de una función que devuelve en un `Par` la media y desviación típica de un array de `double`. 

### Respuesta

```java
public class Par<T, U> {
    private final T primero;
    private final U segundo;

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

public class Estadistica {
    public static Par<Double, Double> calcularMediaYDesviacion(double[] datos) {
        if (datos.length == 0) {
            throw new IllegalArgumentException("Array vacío");
        }

        double suma = 0;
        for (double d : datos) {
            suma += d;
        }
        double media = suma / datos.length;

        double sumaCuadrados = 0;
        for (double d : datos) {
            sumaCuadrados += Math.pow(d - media, 2);
        }
        double desviacion = Math.sqrt(sumaCuadrados / datos.length);

        return new Par<>(media, desviacion);
    }

    public static void main(String[] args) {
        double[] datos = {1.0, 2.0, 3.0, 4.0, 5.0};
        Par<Double, Double> resultado = calcularMediaYDesviacion(datos);
        System.out.println("Media: " + resultado.getPrimero());
        System.out.println("Desviación: " + resultado.getSegundo());
    }
}
```

La clase `Par<T, U>` permite combinar dos tipos distintos de forma segura y reutilizable. El compilador comprueba que el tipo devuelto coincide con el especificado en la instancia.

## 8. En Java, se pueden declarar parámetros de tipo también a nivel de método, no solo a nivel de clase. Pon un ejemplo con un método genérico `seleccionaUno`, que pasados dos objetos del mismo tipo, te devuelva aleatoriamente uno de ellos. Muestra la diferencia de definirlo con dos `Object`, a definirlo con dos parámetros de tipo, en terminos de (i) evitar downcasting y (ii) forzar que ambos objetos sean del mismo tipo. 

### Respuesta

```java
public class Util {
    public static Object seleccionaUnoObject(Object a, Object b) {
        return Math.random() < 0.5 ? a : b;
    }

    public static <T> T seleccionaUno(T a, T b) {
        return Math.random() < 0.5 ? a : b;
    }

    public static void main(String[] args) {
        Object resultado = seleccionaUnoObject("Hola", "Mundo");
        String texto = (String) resultado; // necesita casting

        String resultado2 = seleccionaUno("Hola", "Mundo");
        System.out.println(resultado2);
    }
}
```

Con `Object`, se pierde información de tipo y hay que hacer un casting manual al recuperar el valor, con riesgo de `ClassCastException`. Con parámetros de tipo se evita el downcasting y el compilador fuerza que los dos argumentos sean del mismo tipo, rechazando llamadas como `seleccionaUno("Hola", 42)` en compilación.

## 9. ¿Se pueden establecer restricciones en los parámetros de tipo? Por ejemplo, si quiero definir un tipo genérico `<T>`, ¿puedo decir que tenga que ser, al menos, un número para poder tratarlo como tal? Pon un ejemplo en Java de un `Punto` con dos coordenadas, metodos `getX`, `getY`, y una función `calcularDistanciaA` otro `Punto`. Permite que esas coordenadas sean cualquier tipo de número. Pon dos soluciones: una simplemente creando coordenadas de tipo `Number` y otra añadiendo generics para reforzar el chequeo de tipos y saber exactamente con qué tipo de número trabaja el `Punto`. En este caso y respecto al "type erasure", ¿cuál es el tipo final tras la compilación?

### Respuesta

Sí, se pueden establecer restricciones en los parámetros de tipo con la sintaxis `T extends Tipo`. Esto obliga a que el parámetro sea un subtipo del límite establecido, como `Number`, lo que permite invocar métodos específicos de ese límite.

```java
public class PuntoNumber {
    private Number x;
    private Number y;

    public PuntoNumber(Number x, Number y) {
        this.x = x;
        this.y = y;
    }

    public Number getX() { return x; }
    public Number getY() { return y; }

    public double calcularDistanciaA(PuntoNumber otro) {
        double dx = x.doubleValue() - otro.x.doubleValue();
        double dy = y.doubleValue() - otro.y.doubleValue();
        return Math.sqrt(dx * dx + dy * dy);
    }
}
```

```java
public class PuntoGeneric<T extends Number> {
    private T x;
    private T y;

    public PuntoGeneric(T x, T y) {
        this.x = x;
        this.y = y;
    }

    public T getX() { return x; }
    public T getY() { return y; }

    public double calcularDistanciaA(PuntoGeneric<T> otro) {
        double dx = x.doubleValue() - otro.x.doubleValue();
        double dy = y.doubleValue() - otro.y.doubleValue();
        return Math.sqrt(dx * dx + dy * dy);
    }
}
```

En el primer caso se usa `Number` directamente, lo que permite cualquier número pero pierde información específica del tipo. En el segundo caso, `T extends Number` refuerza el chequeo de tipos para que las coordenadas sean del mismo subtipo de `Number`. Con `type erasure`, el compilador de Java convierte `T` en `Number` en el bytecode, por lo que el tipo final compilado es similar a la versión `Number`, pero con la ventaja de comprobación de tipos en compilación.

## 10. Sobre las soluciones anteriores. Si bien ambas permiten trabajar con distintos tipos de número sin duplicar la clase `Punto`, reflexiona sobre el refuerzo del chequeo de tipos con generics. ¿Permiten ambas crear un punto con una coordenada de tipo entero y la otra coordenada de tipo real? ¿Qué tipo devuelve el `getX` con la solucion sin generics y qué tipo devuelve el que tiene la solución con generics?

### Respuesta

La versión sin generics (`PuntoNumber`) permite crear un punto con una coordenada `Integer` y otra `Double`, porque ambos son subtipos de `Number`. Esto es flexible, pero la seguridad de tipos se debilita, ya que la clase no garantiza que ambas coordenadas sean del mismo tipo.

La versión genérica (`PuntoGeneric<T extends Number>`) no permite mezclar `Integer` y `Double` en una misma instancia cuando se declara `PuntoGeneric<Integer>` o `PuntoGeneric<Double>`. Si se declara `PuntoGeneric<Integer>`, ambas coordenadas deben ser `Integer`. Además, `getX()` en la solución sin generics devuelve `Number`, mientras que en la solución con generics devuelve `T` (por ejemplo `Integer` o `Double`), preservando el tipo concreto y eliminando castings innecesarios.

## 11. Hagamos un ejemplo avanzado. El siguiente código, con interfaz `Punto`, que define un método `calcularDistanciaA(Punto p)`, junto con las implementaciones `Punto2D` y `Punto3D`. Añade generics para asegurarnos que la sobreescritura del método calcular distancia a otro `Punto` siempre es sobre un `Punto` del mismo tipo, evitando `instanceof` y el downcasting.
```java
public interface Punto { 
    public double distanciaA(Punto p); 
} 

public class Punto2D implements Punto { 
     private final double x, y; 
     public Punto2D(double x, double y) { 
        this.x = x; this.y = y; 
    } 

    @Override 
    public double distanciaA(Punto p) { 
        if (p instanceof Punto2D) { 
            Punto2D p2d = (Punto2D) p; 
            return Math.sqrt(Math.pow(x - p2d.x, 2) 
                    + Math.pow(y - p2d.y, 2)); 
        } else { 
            throw new RuntimeException("p debe ser Punto 2D"); 
        } 
    } 
} 
public class Punto3D implements Punto { 
    // Igual que Punto2D, pero con tres coordenadas
    ...
} 
```

### Respuesta

Para evitar `instanceof` y `downcasting`, se puede usar un parámetro genérico recursivo en la interfaz. De este modo, cada implementación garantiza que recibe su propio tipo concreto en el método `distanciaA`.

```java
public interface Punto<T extends Punto<T>> {
    double distanciaA(T p);
}

public class Punto2D implements Punto<Punto2D> {
    private final double x, y;

    public Punto2D(double x, double y) {
        this.x = x;
        this.y = y;
    }

    @Override
    public double distanciaA(Punto2D p) {
        return Math.sqrt(Math.pow(x - p.x, 2)
                + Math.pow(y - p.y, 2));
    }
}

public class Punto3D implements Punto<Punto3D> {
    private final double x, y, z;

    public Punto3D(double x, double y, double z) {
        this.x = x;
        this.y = y;
        this.z = z;
    }

    @Override
    public double distanciaA(Punto3D p) {
        return Math.sqrt(Math.pow(x - p.x, 2)
                + Math.pow(y - p.y, 2)
                + Math.pow(z - p.z, 2));
    }
}
```

Este patrón, conocido como "F-bounded polymorphism", garantiza que la implementación de `distanciaA` en cada clase trabaja con el mismo tipo específico. El compilador rechaza llamadas cruzadas entre `Punto2D` y `Punto3D` sin necesidad de comprobaciones en tiempo de ejecución.

## 12. Dado que `String` es subtipo de `Object`, ¿significa eso que `List<String>` es subtipo de `List<Object>`? ¿Y que `String[]` es subtipo de `Object[]`? Razona por qué la respuesta es diferente en cada caso y qué problema en tiempo de ejecución puede aparecer con los arrays. A partir de estos ejemplos, define qué significa que un tipo genérico sea **covariante**, **contravariante** o **invariante** respecto a su parámetro de tipo.

### Respuesta

No, `List<String>` no es subtipo de `List<Object>`. En Java, los genéricos son invariante; `List<A>` y `List<B>` no guardan relación de subtipado aunque `A` sea subtipo de `B`. Esto evita problemas de tipo al escribir en colecciones tipadas.

Sí, `String[]` es subtipo de `Object[]` porque los arrays en Java son covariantes. Esa covarianza permite asignar `String[]` a `Object[]`, pero puede causar un `ArrayStoreException` en tiempo de ejecución si se intenta insertar un `Integer` en el array asignado a través de la referencia `Object[]`.

Un tipo genérico es **covariante** si preserva la relación de subtipado entre parámetros de tipo (`List<Dog>` sería subtipo de `List<Animal>` si fuese covariante). Es **contravariante** si invierte la relación de subtipado (`List<Animal>` sería subtipo de `List<Dog>` si fuese contravariante). Es **invariante** si no existe relación de subtipado entre `Genérico<A>` y `Genérico<B>` aunque A y B guarden relación.

## 13. Java permite recuperar covarianza y contravarianza en tipos genéricos de forma controlada mediante **wildcards**. ¿Qué es un wildcard (`?`)? Muestra la diferencia entre `List<? extends T>` y `List<? super T>`, indicando en qué casos se usa cada uno. Pon dos ejemplos: (i) un método que reciba una lista de números y calcule su suma, usando `? extends`; (ii) un método que reciba una lista y le añada varios números enteros, usando `? super`.

### Respuesta

Un wildcard (`?`) es un comodín que representa un tipo desconocido en un parámetro genérico. Permite expresar flexibilidad en las relaciones de tipo sin renunciar a las comprobaciones en tiempo de compilación.

`List<? extends T>` es una lista covariante que acepta listas de `T` o de subtipos de `T`; es útil para leer elementos, pero no para escribirlos. `List<? super T>` es una lista contravariante que acepta listas de `T` o de su supertipo; es útil para escribir elementos de tipo `T`, aunque leer devuelva valores de tipo `Object` o el límite superior común.

```java
public static double sumarNumeros(List<? extends Number> numeros) {
    double suma = 0;
    for (Number n : numeros) {
        suma += n.doubleValue();
    }
    return suma;
}

public static void anadirEnteros(List<? super Integer> lista) {
    lista.add(10);
    lista.add(20);
    lista.add(30);
}

public static void main(String[] args) {
    List<Integer> enteros = Arrays.asList(1, 2, 3);
    List<Number> numeros = new ArrayList<>();
    System.out.println(sumarNumeros(enteros));
    anadirEnteros(numeros);
}
```

El wildcard `? extends` se usa para leer de una jerarquía de tipos, mientras que `? super` se usa para escribir en una colección de supertypos. Esto recupera covarianza y contravarianza de forma controlada en Java.

