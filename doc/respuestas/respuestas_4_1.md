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

### Respuesta

En C, se realiza la composición mediante la utilización de estructuras anidadas. Un `struct` puede contener otros `struct` como campos, permitiendo construir estructuras más complejas a partir de componentes simples. En este caso, se define una estructura `Punto` que contiene dos coordenadas, y una estructura `Linea` que está compuesta por dos `Punto`.

```c
#include <stdio.h>
#include <math.h>

typedef struct {
    double x;
    double y;
} Punto;

typedef struct {
    Punto p1;
    Punto p2;
} Linea;

double distancia(Punto p1, Punto p2) {
    double dx = p2.x - p1.x;
    double dy = p2.y - p1.y;
    return sqrt(dx * dx + dy * dy);
}

double longitudLinea(Linea linea) {
    return distancia(linea.p1, linea.p2);
}

int main() {
    Punto p1 = {0, 0};
    Punto p2 = {3, 4};
    Linea linea = {p1, p2};
    
    printf("Distancia: %f\n", distancia(p1, p2));
    printf("Longitud línea: %f\n", longitudLinea(linea));
    
    return 0;
}
```


## 2. Ahora transforma ese ejemplo a orientación a objetos con Java, para tener un primer ejemplo de **composición** en orientación a objetos. Crea una clase `Punto`, y una clase `Linea`. La clase `Punto` debe tener un método para calcular distancia a otro `Punto` y `Linea` debe tener un método para calcular su longitud. Gracias a la ocultación de información, supera a C, garantizando que los puntos sean inmutables, al igual que la línea, que una vez creada, no queremos que se modifique de qué a qué puntos va dicha línea.  

### Respuesta

En Java, la orientación a objetos permite implementar la composición de manera más robusta mediante el uso de clases encapsuladas. Las clases `Punto` y `Linea` se definen de modo que garanticen la inmutabilidad de sus instancias, protegiendo los datos mediante el modificador `private` y evitando la exposición de referencias mutables. Esto es superior a C porque el compilador y el lenguaje refuerzan estas restricciones.

```java
public class Punto {
    private final double x;
    private final double y;
    
    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }
    
    public double getX() { return x; }
    public double getY() { return y; }
    
    public double distancia(Punto otro) {
        double dx = otro.x - this.x;
        double dy = otro.y - this.y;
        return Math.sqrt(dx * dx + dy * dy);
    }
}

public class Linea {
    private final Punto p1;
    private final Punto p2;
    
    public Linea(Punto p1, Punto p2) {
        this.p1 = p1;
        this.p2 = p2;
    }
    
    public Punto getP1() { return p1; }
    public Punto getP2() { return p2; }
    
    public double longitud() {
        return p1.distancia(p2);
    }
}
```


## 3. ¿Qué significa la **multiplicidad** en la composición? En el ejemplo anterior, ¿cuál es la multiplicidad entre `Linea` y `Punto`? Indícalo expresando la multiplicidad en ambas direcciones, de `Linea` a `Punto` y de `Punto` a `Linea`.

### Respuesta

La multiplicidad en la composición expresa la cantidad de instancias de una clase que puede estar relacionada con una instancia de otra clase. Se denota mediante notación como 1:1 (uno a uno), 1:N (uno a muchos) o N:M (muchos a muchos). En el caso del ejemplo, la multiplicidad es bidireccional.

De `Linea` a `Punto`, la multiplicidad es **1:2**, indicando que una línea está siempre compuesta exactamente por dos puntos. De `Punto` a `Linea`, la multiplicidad es **2:N o N**, lo que significa que un punto puede ser parte de cero, una o varias líneas diferentes. Esta asimetría refleja que la línea depende de los puntos para su existencia, aunque un punto puede existir independientemente de cualquier línea.


## 4. ¿Qué significa composición **fuerte** y composición **débil**? ¿Qué consecuencia implica en relación al ciclo de vida de los objetos? Indica a cuál solemos referirnos como **"asociación o agregación"** y a cuál como **"composición"** propiamente.

### Respuesta

La composición fuerte y débil se diferencian principalmente en el ciclo de vida de los objetos componentes. En la composición fuerte, el contenedor es responsable completo de la creación y destrucción de los componentes; su ciclo de vida está ligado al del contenedor, de modo que cuando el contenedor se destruye, sus componentes también desaparecen necesariamente. En la composición débil o agregación, los componentes tienen un ciclo de vida independiente del contenedor; el contenedor simplemente referencia objetos que pueden existir y ser utilizados en otros contextos.

La terminología es importante: la **composición** propiamente dicha se refiere a la relación fuerte, donde hay una relación exclusiva y dependencia total entre contenedor y componentes. La **agregación** o **asociación** se refiere a la composición débil, donde existe una relación menos exclusiva y los componentes pueden ser compartidos o tener vida propia. Por ejemplo, una persona tiene un corazón en composición fuerte (si muere la persona, muere el corazón), pero una persona tiene libros en agregación débil (si la persona desaparece, los libros pueden seguir existiendo).


## 5. Cuando una clase usa a otra al recibirla o devolverla como parámetro en algún método, al hacer `new` dentro de un método, o al usarlas como variables locales, ¿hablamos de composición o de **"dependencia"**?

### Respuesta

En estos casos se habla de **dependencia**, no de composición. La dependencia es una relación más débil y temporal entre clases. Una clase depende de otra cuando hace uso de ella de forma transitoria sin que exista una relación de contenimiento duradero. Esto incluye recibir objetos como parámetros, crear instancias temporales dentro de un método, devolverlas o utilizarlas como variables locales. La característica clave es que la relación no es estructural ni permanente; desaparece cuando el método termina o cuando la referencia se pierde.

La dependencia se diferencia de la composición en que no hay una relación de agregación o pertenencia. Un método puede depender de muchas clases diferentes sin que ninguna de ellas sea componente del objeto que contiene ese método. De hecho, dos clases pueden tener dependencias mutuas sin que ninguna contenga a la otra. Esta es una relación mucho más flexible y débil que las que establece la composición.


## 6. En el ejemplo anterior de línea y punto, programa la relación entre `Linea` y `Punto` de dos formas. Una **como composición fuerte**, donde el ciclo de vida de los puntos está ligado al de Linea y otra **como composición débil**, donde no.

### Respuesta

**Composición Fuerte:** En este caso, la clase `Linea` crea y posee completamente sus puntos. Los puntos se crean dentro del constructor de `Linea` y su existencia depende únicamente de la línea.

```java
// Composición Fuerte
public class LineaFuerte {
    private final Punto p1;
    private final Punto p2;
    
    public LineaFuerte(double x1, double y1, double x2, double y2) {
        this.p1 = new Punto(x1, y1);  // Crea sus propios puntos
        this.p2 = new Punto(x2, y2);
    }
    
    public double longitud() {
        return p1.distancia(p2);
    }
}
```

**Composición Débil:** En este caso, `Linea` recibe puntos ya existentes que fueron creados fuera de la clase. Los puntos pueden existir independientemente de la línea y pueden ser compartidos.

```java
// Composición Débil (Agregación)
public class LineaDebil {
    private final Punto p1;
    private final Punto p2;
    
    public LineaDebil(Punto p1, Punto p2) {
        this.p1 = p1;  // Recibe puntos existentes
        this.p2 = p2;
    }
    
    public double longitud() {
        return p1.distancia(p2);
    }
}

// Uso:
Punto punto1 = new Punto(0, 0);
Punto punto2 = new Punto(3, 4);
LineaDebil linea = new LineaDebil(punto1, punto2);
// Los puntos pueden seguir siendo usados aunque se destruya la línea
```


## 7. En Java, en la composición fuerte, ¿cuando el contenedor destruye los objetos? No se observa que `Linea` destruya los `Punto` explícitamente, ¿Por qué?

### Respuesta

En Java, no es necesario destruir explícitamente los objetos en el contenedor porque el lenguaje incluye un recolector de basura automático (garbage collector). Cuando un objeto (como una instancia de `Linea`) sale del alcance y no hay más referencias a él, tanto el objeto como todas las referencias que contenía se marcan para ser recolectadas automáticamente. No se requiere escribir código destructivo como se haría en C++ con el destructor.

El recolector de basura se encarga de liberar memoria de los objetos que ya no tienen referencias vivas en el programa. En una composición fuerte, cuando `Linea` es destruida y no hay otras referencias a sus `Punto` internos, estos también serán recolectados automáticamente. Este mecanismo es una ventaja fundamental de Java respecto a C, donde la liberación de memoria debe ser explícita y manual, lo que puede causar fugas de memoria si no se gestiona correctamente.


## 8. Pon un ejemplo de composicion débil entre un departamento que tiene varios profesores. Implementa dos composiciones a la vez: entre el departamento y todos sus profesores y entre el departamento y su director, que es un profesor del departamento. Siempre debe haber un director en el departamento desde el inicio. Lanza excepciones si se viola la invariante. Emplea arrays primitivos de Java, estilo `Profesor[]`, con máximo 50, pero no rompas la encapsulación, no desveles que estás empleando un array, permite añadir un `Profesor` al final de la lista, y eliminar un profesor dada su posición. Da acceso a los profesores con un método para saber cuántos hay y otro para obtener un profesor por posición. El director se puede cambiar por otro profesor del departamento. Sin embargo, ten en cuenta esta invariante de clase: el director debe formar siempre parte de la lista de profesores, es decir, ten cuidado al cambiar el director o al eliminar un profesor.

### Respuesta

```java
public class Profesor {
    private String nombre;
    
    public Profesor(String nombre) {
        this.nombre = nombre;
    }
    
    public String getNombre() {
        return nombre;
    }
}

public class Departamento {
    private static final int MAX_PROFESORES = 50;
    private Profesor[] profesores;
    private int cantidad;
    private Profesor director;
    
    public Departamento(String nombre, Profesor director) 
            throws IllegalArgumentException {
        if (director == null) {
            throw new IllegalArgumentException(
                "El departamento debe tener un director");
        }
        this.profesores = new Profesor[MAX_PROFESORES];
        this.cantidad = 0;
        this.director = director;
        // Añadir el director como primer profesor
        this.profesores[0] = director;
        this.cantidad = 1;
    }
    
    public void agregarProfesor(Profesor profesor) 
            throws IllegalArgumentException {
        if (profesor == null) {
            throw new IllegalArgumentException(
                "No se puede añadir un profesor nulo");
        }
        if (cantidad >= MAX_PROFESORES) {
            throw new IllegalArgumentException(
                "El departamento está lleno (máximo 50 profesores)");
        }
        profesores[cantidad] = profesor;
        cantidad++;
    }
    
    public void eliminarProfesor(int posicion) 
            throws IndexOutOfBoundsException, IllegalArgumentException {
        if (posicion < 0 || posicion >= cantidad) {
            throw new IndexOutOfBoundsException(
                "Posición fuera de rango");
        }
        // No permitir eliminar el director
        if (profesores[posicion] == director) {
            throw new IllegalArgumentException(
                "No se puede eliminar al director del departamento");
        }
        // Desplazar elementos
        for (int i = posicion; i < cantidad - 1; i++) {
            profesores[i] = profesores[i + 1];
        }
        profesores[cantidad - 1] = null;
        cantidad--;
    }
    
    public int contarProfesores() {
        return cantidad;
    }
    
    public Profesor getProfesor(int posicion) 
            throws IndexOutOfBoundsException {
        if (posicion < 0 || posicion >= cantidad) {
            throw new IndexOutOfBoundsException(
                "Posición fuera de rango");
        }
        return profesores[posicion];
    }
    
    public Profesor getDirector() {
        return director;
    }
    
    public void cambiarDirector(Profesor nuevoDirector) 
            throws IllegalArgumentException {
        if (nuevoDirector == null) {
            throw new IllegalArgumentException(
                "El director no puede ser nulo");
        }
        // Verificar que el nuevo director es profesor del departamento
        boolean encontrado = false;
        for (int i = 0; i < cantidad; i++) {
            if (profesores[i] == nuevoDirector) {
                encontrado = true;
                break;
            }
        }
        if (!encontrado) {
            throw new IllegalArgumentException(
                "El nuevo director debe ser un profesor del departamento");
        }
        this.director = nuevoDirector;
    }
}
```


## 9. En Java, existen también `List`, cambia y muestra cómo sería el código anterior empleando `List` en vez de arrays primitivos. ¿Qué parte del código original te has ahorrado? Además, fíjate en el método `getProfesor(int pos)`: si en su lugar existiera un método que devolviera todos los profesores a la vez, ¿qué problema tendría devolver directamente la lista interna? ¿Cómo lo resolverías?

### Respuesta

Usando `List`, el código se simplifica significativamente porque `List` gestiona automáticamente el crecimiento dinámico, eliminando la necesidad de manejo manual de arrays. Se elimina completamente la comprobación de límite máximo, el desplazamiento manual de elementos y el rastreo manual del contador. Además, `List` proporciona métodos convenientes como `contains()` y `remove()` que hacen innecesario escribir lógica adicional.

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public class DepartamentoList {
    private List<Profesor> profesores;
    private Profesor director;
    
    public DepartamentoList(Profesor director) 
            throws IllegalArgumentException {
        if (director == null) {
            throw new IllegalArgumentException(
                "El departamento debe tener un director");
        }
        this.profesores = new ArrayList<>();
        this.director = director;
        this.profesores.add(director);
    }
    
    public void agregarProfesor(Profesor profesor) 
            throws IllegalArgumentException {
        if (profesor == null) {
            throw new IllegalArgumentException(
                "No se puede añadir un profesor nulo");
        }
        this.profesores.add(profesor);
    }
    
    public void eliminarProfesor(int posicion) 
            throws IndexOutOfBoundsException, IllegalArgumentException {
        Profesor profesor = this.profesores.get(posicion);
        if (profesor == director) {
            throw new IllegalArgumentException(
                "No se puede eliminar al director del departamento");
        }
        this.profesores.remove(posicion);
    }
    
    public int contarProfesores() {
        return this.profesores.size();
    }
    
    public Profesor getProfesor(int posicion) {
        return this.profesores.get(posicion);
    }
    
    public Profesor getDirector() {
        return director;
    }
    
    public void cambiarDirector(Profesor nuevoDirector) 
            throws IllegalArgumentException {
        if (!this.profesores.contains(nuevoDirector)) {
            throw new IllegalArgumentException(
                "El nuevo director debe ser un profesor del departamento");
        }
        this.director = nuevoDirector;
    }
    
    // Retornar una copia para evitar modificaciones externas
    public List<Profesor> getProfesores() {
        return Collections.unmodifiableList(
            new ArrayList<>(this.profesores));
    }
}
```

Si se devolviera directamente la lista interna mediante `return this.profesores;`, el problema sería que código externo podría modificar la lista sin pasar por los métodos de validación de la clase, violando las invariantes. Por ejemplo, se podría eliminar directamente al director. La solución es devolver una **copia defensiva** de la lista (como se muestra con `new ArrayList<>(this.profesores)`) o una vista inmutable usando `Collections.unmodifiableList()`, impidiendo que se modifique la estructura interna desde fuera.


## 10. Al igual que ocurre con las excepciones en Java, que pueden encerrar causas (que son excepciones), de forma recursiva, suponen un tipo especial de composiciones, denominadas composiciones recursivas. Pon un ejemplo en Java de una `Persona`, que sea inmutable, y que tiene una madre, que es otra `Persona`. Haz un main con un ejemplo de uso con una familia de personas, desde el nieto hasta la abuela. Enumera algún otro ejemplo clásico de composiciones recursivas.

### Respuesta

Las composiciones recursivas ocurren cuando una clase contiene una instancia de sí misma (directa o indirectamente). La clave es que la referencia sea opcional (generalmente null para el caso base) para evitar bucles infinitos. En el ejemplo de `Persona`, cada persona tiene una referencia a su madre, que es otra `Persona`, formando una cadena que termina cuando alguien no tiene madre registrada.

```java
public class Persona {
    private final String nombre;
    private final Persona madre;  // Composición recursiva
    
    public Persona(String nombre, Persona madre) {
        this.nombre = nombre;
        this.madre = madre;
    }
    
    public Persona(String nombre) {
        this(nombre, null);
    }
    
    public String getNombre() {
        return nombre;
    }
    
    public Persona getMadre() {
        return madre;
    }
    
    // Método para imprimir la genealogía
    public void imprimirArbolGenealogia(int nivel) {
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < nivel; i++) {
            sb.append("  ");
        }
        System.out.println(sb.toString() + nombre);
        if (madre != null) {
            madre.imprimirArbolGenealogia(nivel + 1);
        }
    }
}

public class Main {
    public static void main(String[] args) {
        // Construir la familia desde la abuela hacia abajo
        Persona abuela = new Persona("Abuela María");
        Persona madre = new Persona("Madre Rosa", abuela);
        Persona nieto = new Persona("Nieto Juan", madre);
        
        System.out.println("Árbol genealógico:");
        nieto.imprimirArbolGenealogia(0);
    }
}
```

Otros ejemplos clásicos de composiciones recursivas incluyen: estructuras de archivos y directorios (un directorio contiene archivos y otros directorios), árboles genealógicos, representación de expresiones matemáticas (una expresión contiene operadores y otras expresiones), gráficos de dependencias, estructuras de datos como árboles binarios y listas enlazadas (un nodo contiene datos y referencias a otros nodos), y menús con submenús en interfaces gráficas.

## 11. ¿Qué son las relaciones de composición "bidireccionales"? ¿Qué habría que hacer para implementar este tipo de relación en el ejemplo de `Profesor` y `Departamento`?

### Respuesta

Las relaciones bidireccionales son aquellas donde ambas clases mantienen referencias la una a la otra. Mientras que en el ejemplo anterior el `Departamento` conoce a sus `Profesor`es pero los profesores no conocen su departamento, en una relación bidireccional cada `Profesor` también mantendría una referencia a su `Departamento`. Esto permite navegar la relación en ambas direcciones, pero introduce complejidad porque se debe mantener la consistencia: cuando se añade un profesor al departamento, el profesor debe ser informado de su nuevo departamento, y esta sincronización debe hacerse cuidadosamente.

```java
public class ProfesorBi {
    private String nombre;
    private Departamento departamento;
    
    public ProfesorBi(String nombre) {
        this.nombre = nombre;
        this.departamento = null;
    }
    
    public String getNombre() { return nombre; }
    public Departamento getDepartamento() { return departamento; }
    
    // Solo se debe llamar desde Departamento
    protected void setDepartamento(Departamento dept) {
        this.departamento = dept;
    }
}

public class DepartamentoBi {
    private List<ProfesorBi> profesores;
    private ProfesorBi director;
    
    public DepartamentoBi(ProfesorBi director) {
        this.profesores = new ArrayList<>();
        this.director = director;
        director.setDepartamento(this);
        this.profesores.add(director);
    }
    
    public void agregarProfesor(ProfesorBi profesor) {
        if (profesor == null) {
            throw new IllegalArgumentException(
                "No se puede añadir un profesor nulo");
        }
        this.profesores.add(profesor);
        profesor.setDepartamento(this);
    }
    
    public void eliminarProfesor(int posicion) {
        ProfesorBi profesor = this.profesores.get(posicion);
        if (profesor == director) {
            throw new IllegalArgumentException(
                "No se puede eliminar al director");
        }
        this.profesores.remove(posicion);
        profesor.setDepartamento(null);
    }
    
    public int contarProfesores() {
        return this.profesores.size();
    }
    
    public ProfesorBi getProfesor(int posicion) {
        return this.profesores.get(posicion);
    }
}
```

La implementación requiere sincronización cuidadosa: cuando se modifica un lado de la relación (por ejemplo, al añadir un profesor), se debe actualizar automáticamente el otro lado. Para mantener la encapsulación, se usa un método `protected` que solo el departamento puede invocar. Las relaciones bidireccionales deben usarse prudentemente ya que pueden complicar el código y crear ciclos de referencias que afecten a la recolección de basura.
