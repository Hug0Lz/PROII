<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Clases y Objetos". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: ninguno.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 1. Clases y objetos

## 1. ¿Cuáles son las cuatro características básicas de la programación orientada a objetos? Describe brevemente cada una

### Respuesta

Las cuatro características básicas son **abstracción**, **encapsulamiento**, **herencia** y **polimorfismo**. Estas ideas permiten organizar el software de forma más cercana a cómo se modelan los elementos del mundo real, pero llevados al terreno de la programación.

La **abstracción** consiste en representar solo los aspectos relevantes de un objeto, ignorando los detalles innecesarios. El **encapsulamiento** implica agrupar datos y operaciones relacionadas dentro de una misma unidad (la clase) y controlar el acceso a esos datos.

La **herencia** permite crear nuevas clases a partir de otras existentes, reutilizando comportamiento. El **polimorfismo** permite tratar objetos de distintas clases de forma uniforme, siempre que compartan una interfaz o comportamiento común.

---

## 2. Cita cuatro lenguajes populares que permitan la programación orientada a objetos

### Respuesta

Existen muchos lenguajes que soportan programación orientada a objetos. Entre los más conocidos se encuentran **Java**, **C++**, **C#** y **Python**. Todos ellos permiten definir clases, crear objetos y usar conceptos como métodos y atributos.

**Java** y **C#** fueron diseñados con la POO como base principal. **C++** añade la POO sobre el lenguaje C, por lo que combina programación estructurada y orientada a objetos. **Python** también soporta POO, aunque su sintaxis es más flexible y menos estricta.

---

## 3. Los paradigmas anteriores a la POO, ¿Qué es la programación estructurada? y, todavía mejor, ¿Qué es la programación modular?

### Respuesta

La **programación estructurada** es un paradigma que organiza el código mediante estructuras de control como secuencia, selección (`if`) e iteración (`for`, `while`). Su objetivo principal es evitar el uso desordenado de saltos como `goto`, favoreciendo un flujo de control claro.

La **programación modular** va un paso más allá y divide el programa en partes independientes llamadas módulos. Cada módulo agrupa funciones relacionadas y puede desarrollarse y mantenerse por separado.

Mientras la programación estructurada se centra en cómo fluye la ejecución, la modular se centra en cómo se organiza el código en bloques lógicos reutilizables.

---

## 4. ¿Qué tres elementos definen a un objeto en programación orientada a objetos?

### Respuesta

Un objeto se define por **estado**, **comportamiento** e **identidad**. Estos tres aspectos permiten diferenciar un objeto de otro, incluso si son del mismo tipo.

El **estado** está formado por los valores de sus atributos en un momento dado. El **comportamiento** viene dado por los métodos que puede ejecutar. La **identidad** es lo que hace que un objeto sea único en memoria, aunque tenga los mismos valores que otro.

---

## 5. ¿Qué es una clase? ¿Es lo mismo que un objeto? ¿Qué es una instancia? ¿Todos los lenguajes orientados a objetos manejan el concepto de clase?

### Respuesta

Una **clase** es un molde o plantilla que define atributos y métodos comunes. No representa un elemento concreto, sino la definición general de cómo serán los objetos de ese tipo.

Un **objeto** es una realización concreta de una clase. Una **instancia** es, precisamente, ese objeto creado a partir de la clase. Por tanto, clase y objeto no son lo mismo: uno define y el otro existe en memoria.

No todos los lenguajes orientados a objetos usan clases de forma obligatoria. Algunos, como Java, sí se basan totalmente en clases, mientras que otros como JavaScript permiten modelos basados en prototipos.

---

## 6. ¿Dónde se almacenan en memoria los objetos? ¿Es igual en todos los lenguajes? ¿Qué es la recolección de basura?

### Respuesta

En muchos lenguajes orientados a objetos, los objetos se almacenan en una zona de memoria llamada **heap** (montículo). Las variables que apuntan a ellos suelen estar en la pila (stack), pero el objeto en sí vive en el heap.

Esto no es exactamente igual en todos los lenguajes. Algunos permiten objetos en la pila, otros gestionan la memoria de forma más automática. La forma concreta depende del lenguaje y su implementación.

La **recolección de basura** es un mecanismo automático que libera la memoria de objetos que ya no se usan. Evita que el programador tenga que liberar manualmente cada bloque de memoria, reduciendo errores como fugas.

---

## 7. ¿Qué es un método? ¿Qué es la sobrecarga de métodos?

### Respuesta

Un **método** es una función definida dentro de una clase que describe el comportamiento de los objetos de esa clase. Puede acceder a los atributos del objeto y realizar operaciones sobre ellos.

La **sobrecarga de métodos** ocurre cuando existen varios métodos con el mismo nombre pero distinta lista de parámetros. El compilador decide cuál usar según los argumentos proporcionados en la llamada.

---

## 8. Ejemplo mínimo de clase en Java

### Respuesta

```java
class Punto {
    double x;
    double y;

    double calculaDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }
}

public class Principal {
    public static void main(String[] args) {
        Punto p = new Punto();
        p.x = 3;
        p.y = 4;
        System.out.println(p.calculaDistanciaAOrigen());
    }
}
```

---

## 9. ¿Cuál es el punto de entrada en un programa en Java?

### Respuesta

El punto de entrada es el método `public static void main(String[] args)`. Es el primero que ejecuta la máquina virtual cuando se inicia el programa.

`static` indica que el método pertenece a la clase y no a un objeto concreto. No se usa solo en `main`, también sirve para atributos y métodos comunes a todas las instancias.

Combinado con `final`, indica que un valor no puede modificarse. Se usa para constantes, por ejemplo `static final` para valores compartidos inmutables.

---

## 10. Compilación y ejecución en Java

### Respuesta

Para compilar se usa `javac Programa.java`, lo que genera archivos `.class`. Para ejecutar se usa `java Programa`, indicando la clase con `main`.

Java es un lenguaje compilado a un formato intermedio. Ese formato es el **byte-code**, que no es código máquina directo.

La **máquina virtual de Java (JVM)** interpreta o compila en tiempo de ejecución ese byte-code. Los archivos `.class` contienen ese byte-code.

---

## 11. ¿Qué es `new`?

### Respuesta

`new` se usa para crear un objeto en memoria. Reserva espacio y llama al **constructor**, que es un método especial que inicializa el objeto.

El constructor tiene el mismo nombre que la clase y no tiene tipo de retorno.

```java
class Empleado {
    String dni;
    String nombre;
    String apellidos;

    Empleado(String dni, String nombre, String apellidos) {
        this.dni = dni;
        this.nombre = nombre;
        this.apellidos = apellidos;
    }
}
```

---

## 12. ¿Qué es `this`?

### Respuesta

`this` es una referencia al objeto actual, es decir, al objeto que está ejecutando el método. Se usa para diferenciar atributos de parámetros con el mismo nombre.

No en todos los lenguajes se llama igual, pero muchos tienen un concepto similar.

```java
double calculaDistanciaAOrigen() {
    return Math.sqrt(this.x * this.x + this.y * this.y);
}
```

---

## 13. Nuevo método `distanciaA`

### Respuesta

```java
double distanciaA(Punto otro) {
    double dx = this.x - otro.x;
    double dy = this.y - otro.y;
    return Math.sqrt(dx * dx + dy * dy);
}
```

Este método recibe otro objeto `Punto` y calcula la distancia entre ambos usando sus atributos.

---

## 14. Paso de parámetros

### Respuesta

En Java, los objetos se pasan por **valor de la referencia**. Se copia la referencia, no el objeto. Si se modifican los atributos del objeto recibido, los cambios sí afectan al objeto original.

En cambio, los tipos primitivos como `int` se pasan por copia del valor. Si se modifica dentro del método, el valor original fuera no cambia.

---

## 15. ¿Qué es `toString()`?

### Respuesta

`toString()` es un método que devuelve una representación en texto del objeto. Se usa al imprimir objetos.

Otros lenguajes tienen mecanismos similares.

```java
public String toString() {
    return "(" + x + ", " + y + ")";
}
```

---

## 16. ¿Una clase es como un `struct` en C?

### Respuesta

Un `struct` en C agrupa datos, igual que una clase, pero no incluye directamente comportamiento asociado. Las funciones no están unidas al `struct`.

Para parecerse a una clase, el `struct` necesitaría funciones asociadas, control de acceso y mecanismos como constructores.

---

## 17. Emular clase con `struct` en C

### Respuesta

```c
#include <math.h>

typedef struct {
    double x;
    double y;
} Punto;

double calculaDistanciaAOrigen(Punto* p) {
    return sqrt(p->x * p->x + p->y * p->y);
}
```

Aquí `this` se reemplaza por un puntero al `struct` pasado como parámetro. Ya no es implícito, hay que pasarlo manualmente.
