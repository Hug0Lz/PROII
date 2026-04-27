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

### Respuesta

El polimorfismo es la capacidad de objetos de diferentes tipos de responder a la misma llamada de método de formas diferentes. El término proviene del griego "poli" (muchos) y "morfos" (forma), literalmente "muchas formas". Sirve para escribir código genérico que funcione con múltiples tipos sin necesidad de conocer los detalles específicos de cada uno, facilitando la extensibilidad y el mantenimiento del código. Permite que código cliente trabaje con referencias de tipos generales y que cada tipo concreto implemente el comportamiento que considera apropiado.

La sobreescritura de métodos (*overriding*) es el mecanismo mediante el cual una subclase proporciona una implementación específica de un método que ya existe en la superclase. Cuando se sobreescribe un método, la versión de la subclase reemplaza completamente a la de la superclase cuando se invoca a través de una instancia de la subclase. Esta es la base del polimorfismo: permite que el mismo método tenga comportamientos diferentes dependiendo del tipo real del objeto, no del tipo de la referencia.

El polimorfismo es la capacidad de objetos de diferentes tipos de responder a la misma llamada de método de formas diferentes. El término proviene del griego "poli" (muchos) y "morfos" (forma), literalmente "muchas formas". Sirve para escribir código genérico que funcione con múltiples tipos sin necesidad de conocer los detalles específicos de cada uno, facilitando la extensibilidad y el mantenimiento del código. Permite que código cliente trabaje con referencias de tipos generales y que cada tipo concreto implemente el comportamiento que considera apropiado.

La sobreescritura de métodos (*overriding*) es el mecanismo mediante el cual una subclase proporciona una implementación específica de un método que ya existe en la superclase. Cuando se sobreescribe un método, la versión de la subclase reemplaza completamente a la de la superclase cuando se invoca a través de una instancia de la subclase. Esta es la base del polimorfismo: permite que el mismo método tenga comportamientos diferentes dependiendo del tipo real del objeto, no del tipo de la referencia.


## 2. ¿En qué consiste la **"ligadura dinámica"** o **"enlace tardío"**? ¿qué relación tiene con el polimorfismo? ¿hay que indicarlos explícitamente al programar o depende esto del lenguaje? Compara C++ y Java. Indicalo después también para Python.

### Respuesta

La ligadura dinámica o enlace tardío es el mecanismo por el cual se determina en tiempo de ejecución (en lugar de en tiempo de compilación) qué método se debe invocar. Es decir, la decisión sobre qué versión de un método se ejecuta se toma según el tipo real del objeto en tiempo de ejecución, no según el tipo de la referencia. Esta es la esencia del polimorfismo: aunque el código llame a un método a través de una referencia de la superclase, se ejecuta el método de la subclase correspondiente.

En Java, la ligadura dinámica es el comportamiento por defecto para métodos no-estáticos. No es necesario hacer nada especial para habilitarla; sucede automáticamente. En C++, esto no es así: los métodos se resuelven estáticamente por defecto para eficiencia, siendo necesario marcar explícitamente los métodos como `virtual` para que se use ligadura dinámica. En Python, la ligadura dinámica es el comportamiento natural y automático, sin necesidad de palabras clave, porque Python es un lenguaje dinámicamente tipado. Esto hace que el polimorfismo sea más "fácil" en Python pero menos seguro a nivel de tipos que en Java.

La ligadura dinámica o enlace tardío es el mecanismo por el cual se determina en tiempo de ejecución (en lugar de en tiempo de compilación) qué método se debe invocar. Es decir, la decisión sobre qué versión de un método se ejecuta se toma según el tipo real del objeto en tiempo de ejecución, no según el tipo de la referencia. Esta es la esencia del polimorfismo: aunque el código llame a un método a través de una referencia de la superclase, se ejecuta el método de la subclase correspondiente.

En Java, la ligadura dinámica es el comportamiento por defecto para métodos no-estáticos. No es necesario hacer nada especial para habilitarla; sucede automáticamente. En C++, esto no es así: los métodos se resuelven estáticamente por defecto para eficiencia, siendo necesario marcar explícitamente los métodos como `virtual` para que se use ligadura dinámica. En Python, la ligadura dinámica es el comportamiento natural y automático, sin necesidad de palabras clave, porque Python es un lenguaje dinámicamente tipado. Esto hace que el polimorfismo sea más "fácil" en Python pero menos seguro a nivel de tipos que en Java.




## 3. Pon un ejemplo sencillo en Java, de un `Soldado`, con un método `saluda`, con dos subclases: `Zapador` y `Artillero`, donde `Zapador` sobreescribe el método `saludar`, sustituyendo por completo su comportamiento. Ilustra el funcionamiento del polimorfismo creando un array de `Soldados` de dos tipos y luego recorriéndolo empleando referencias de tipo `Soldado` y llamando a `saludar`.

### Respuesta

```java
public class Soldado {
    private String nombre;
    
    public Soldado(String nombre) {
        this.nombre = nombre;
    }
    
    public void saludar() {
        System.out.println("Soldado " + nombre + " saluda.");
    }
}

public class Artillero extends Soldado {
    public Artillero(String nombre) {
        super(nombre);
    }
    // No sobreescribe, usa el método de Soldado
}

public class Zapador extends Soldado {
    public Zapador(String nombre) {
        super(nombre);
    }
    
    @Override
    public void saludar() {
        System.out.println("¡ZAPADOR A VUESTRO SERVICIO!");
    }
}

public class Main {
    public static void main(String[] args) {
        Soldado[] ejercito = new Soldado[3];
        ejercito[0] = new Artillero("Juan");
        ejercito[1] = new Zapador("María");
        ejercito[2] = new Artillero("Carlos");
        
        // Polimorfismo: mismo método, diferentes comportamientos
        for (Soldado s : ejercito) {
            s.saludar();
        }
    }
}

// Salida:
// Soldado Juan saluda.
// ¡ZAPADOR A VUESTRO SERVICIO!
// Soldado Carlos saluda.
```

```java
public class Soldado {
    private String nombre;
    
    public Soldado(String nombre) {
        this.nombre = nombre;
    }
    
    public void saludar() {
        System.out.println("Soldado " + nombre + " saluda.");
    }
}

public class Artillero extends Soldado {
    public Artillero(String nombre) {
        super(nombre);
    }
    // No sobreescribe, usa el método de Soldado
}

public class Zapador extends Soldado {
    public Zapador(String nombre) {
        super(nombre);
    }
    
    @Override
    public void saludar() {
        System.out.println("¡ZAPADOR A VUESTRO SERVICIO!");
    }
}

public class Main {
    public static void main(String[] args) {
        Soldado[] ejercito = new Soldado[3];
        ejercito[0] = new Artillero("Juan");
        ejercito[1] = new Zapador("María");
        ejercito[2] = new Artillero("Carlos");
        
        // Polimorfismo: mismo método, diferentes comportamientos
        for (Soldado s : ejercito) {
            s.saludar();
        }
    }
}

// Salida:
// Soldado Juan saluda.
// ¡ZAPADOR A VUESTRO SERVICIO!
// Soldado Carlos saluda.
```


## 4. Si sobreescribo un método, ¿puedo invocar el método base para trabajar a partir de su resultado? Haz que zapador cambie ligeramente la forma de saludar, que salude de forma normal, tal cual hace el soldado base, pero que además añada un "ZAPADOR A SUS ORDENES" ¿qué palabra clave del lenguaje has usado para invocar al método de la clase base?

### Respuesta

Sí es completamente posible invocar el método de la clase base desde la subclase usando la palabra clave `super`. Esto permite reutilizar la lógica de la superclase y extenderla con comportamiento adicional, una técnica muy común en el polimorfismo. La palabra clave `super` actúa como una referencia explícita a la clase base, permitiendo acceder a sus métodos incluso cuando han sido sobreescritos.

```java
public class Zapador extends Soldado {
    public Zapador(String nombre) {
        super(nombre);
    }
    
    @Override
    public void saludar() {
        super.saludar();  // Invoca el método de Soldado
        System.out.println("¡ZAPADOR A SUS ÓRDENES!");
    }
}

// Salida cuando se invoca saludar() en un Zapador:
// Soldado [nombre] saluda.
// ¡ZAPADOR A SUS ÓRDENES!
```

Sí es completamente posible invocar el método de la clase base desde la subclase usando la palabra clave `super`. Esto permite reutilizar la lógica de la superclase y extenderla con comportamiento adicional, una técnica muy común en el polimorfismo. La palabra clave `super` actúa como una referencia explícita a la clase base, permitiendo acceder a sus métodos incluso cuando han sido sobreescritos.

```java
public class Zapador extends Soldado {
    public Zapador(String nombre) {
        super(nombre);
    }
    
    @Override
    public void saludar() {
        super.saludar();  // Invoca el método de Soldado
        System.out.println("¡ZAPADOR A SUS ÓRDENES!");
    }
}

// Salida cuando se invoca saludar() en un Zapador:
// Soldado [nombre] saluda.
// ¡ZAPADOR A SUS ÓRDENES!
```


## 5. Al sobreescribir un método en Java, ¿qué restricciones existen sobre los tipos de los parámetros y el tipo de retorno? ¿Qué diferencia hay entre sobreescritura (*overriding*) y sobrecarga (*overloading*)? ¿Para qué sirve la anotación `@Override` y por qué es recomendable usarla siempre?

### Respuesta

Al sobreescribir un método, los tipos de los parámetros y el tipo de retorno deben ser exactamente iguales a los del método en la superclase (o en el caso del tipo de retorno, puede ser un subtipo, lo que se llama "return type covariance"). Si los parámetros son diferentes, no se está sobreescribiendo sino creando un nuevo método, lo que sería una sobrecarga. Los modificadores de acceso no pueden ser más restrictivos (si la superclase tiene `public`, la subclase también debe serlo).

La sobrecarga (*overloading*) es diferente: es cuando la misma clase tiene múltiples métodos con el mismo nombre pero diferentes parámetros. Se resuelve en tiempo de compilación según los tipos de los argumentos. La sobreescritura (*overriding*) es cuando una subclase redefine un método heredado, resuelto en tiempo de ejecución mediante ligadura dinámica. La anotación `@Override` es una "anotación" que advierte al compilador que se intenta sobreescribir un método. Si hay un error (por ejemplo, parámetros diferentes), el compilador lanzará un error. Es recomendable usarla siempre porque previene errores accidentales y hace el código más legible y mantenible.

```java
public class Soldado {
    public void saludar() { }
    public void atacar(String objetivo) { }
}

public class Zapador extends Soldado {
    @Override  // Correcta: misma firma
    public void saludar() { }
    
    @Override  // Correcta: sobrecarga, no sobreescritura
    public void atacar(String objetivo, int fuerza) { }
}
```

Al sobreescribir un método, los tipos de los parámetros y el tipo de retorno deben ser exactamente iguales a los del método en la superclase (o en el caso del tipo de retorno, puede ser un subtipo, lo que se llama "return type covariance"). Si los parámetros son diferentes, no se está sobreescribiendo sino creando un nuevo método, lo que sería una sobrecarga. Los modificadores de acceso no pueden ser más restrictivos (si la superclase tiene `public`, la subclase también debe serlo).

La sobrecarga (*overloading*) es diferente: es cuando la misma clase tiene múltiples métodos con el mismo nombre pero diferentes parámetros. Se resuelve en tiempo de compilación según los tipos de los argumentos. La sobreescritura (*overriding*) es cuando una subclase redefine un método heredado, resuelto en tiempo de ejecución mediante ligadura dinámica. La anotación `@Override` es una "anotación" que advierte al compilador que se intenta sobreescribir un método. Si hay un error (por ejemplo, parámetros diferentes), el compilador lanzará un error. Es recomendable usarla siempre porque previene errores accidentales y hace el código más legible y mantenible.

```java
public class Soldado {
    public void saludar() { }
    public void atacar(String objetivo) { }
}

public class Zapador extends Soldado {
    @Override  // Correcta: misma firma
    public void saludar() { }
    
    @Override  // Correcta: sobrecarga, no sobreescritura
    public void atacar(String objetivo, int puerza) { }
}
```


## 6. Entonces, cuando se estudia Java, ¿se emplea el polimorfismo desde el principio? Por ejemplo, sobreescribiendo `toString` o sobreescribiendo `equals`, ¿ya estoy usando polimorfismo?

### Respuesta

Sí, desde el momento en que se crea la primera clase en Java que hereda implícitamente de `Object`, se está usando polimorfismo, aunque no sea evidente. Cuando se sobreescribe `toString()` o `equals()`, se está sobreescribiendo métodos heredados de `Object`, y cuando el código de framework de Java llama a estos métodos a través de referencias de tipo `Object`, la ligadura dinámica determina qué versión ejecutar. Entonces sí, ya se está usando polimorfismo desde los ejemplos más básicos.

String, ArrayList y muchas otras clases de la API estándar de Java están diseñadas aprovechando polimorfismo. Por ejemplo, cuando se llama a `toString()` en un objeto sin saber su tipo exacto, la ligadura dinámica asegura que se ejecute la versión correcta. El código de bibliotecas como las colecciones depende completamente del polimorfismo de `hashCode()` y `equals()`. De esta forma, el polimorfismo no es un concepto avanzado sino uno fundamental, presente desde el inicio en Java.

Sí, desde el momento en que se crea la primera clase en Java que hereda implícitamente de `Object`, se está usando polimorfismo, aunque no sea evidente. Cuando se sobreescribe `toString()` o `equals()`, se está sobreescribiendo métodos heredados de `Object`, y cuando el código de framework de Java llama a estos métodos a través de referencias de tipo `Object`, la ligadura dinámica determina qué versión ejecutar. Entonces sí, ya se está usando polimorfismo desde los ejemplos más básicos.

String, ArrayList y muchas otras clases de la API estándar de Java están diseñadas aprovechando polimorfismo. Por ejemplo, cuando se llama a `toString()` en un objeto sin saber su tipo exacto, la ligadura dinámica asegura que se ejecute la versión correcta. El código de bibliotecas como las colecciones depende completamente del polimorfismo de `hashCode()` y `equals()`. De esta forma, el polimorfismo no es un concepto avanzado sino uno fundamental, presente desde el inicio en Java.


## 7. ¿Qué es una **"clase abstracta"**? ¿Qué es un **"método abstracto"**? ¿Puedo crear instancias de una clase abstracta? Pongamos un ejemplo en Java: Redefinamos `Soldado`, hagamos que, además del método `saluda` que ya tenía, tenga un método `atacar`, que sea abstracto y que cada tipo de soldado haga su acción cuando se le pida atacar. ¿Donde debemos poner `abstract`?

### Respuesta

Una clase abstracta es una clase que no puede ser instanciada directamente y existe para ser heredada. Sirve como plantilla que define qué métodos deben implementar sus subclases. Un método abstracto es un método sin implementación (solo tiene la firma) que obliga a todas las subclases a proporcionarle una implementación. No se puede crear una instancia de una clase abstracta; intentarlo resultaría en un error de compilación. Si una subclase hereda de una clase abstracta sin implementar todos sus métodos abstractos, la subclase también debe declararse como abstracta.

La palabra clave `abstract` debe colocarse en dos lugares: antes de la palabra clave `class` para la clase abstracta, y también antes del tipo de retorno del método abstracto. Esto indica al compilador que esta clase no puede ser instanciada y que sus subclases tienen la responsabilidad de implementar los métodos abstractos.

```java
public abstract class Soldado {
    protected String nombre;
    
    public Soldado(String nombre) {
        this.nombre = nombre;
    }
    
    public void saludar() {
        System.out.println("Soldado " + nombre + " saluda.");
    }
    
    // Método abstracto - sin implementación
    public abstract void atacar();
}

public class Zapador extends Soldado {
    public Zapador(String nombre) {
        super(nombre);
    }
    
    @Override
    public void atacar() {
        System.out.println(nombre + " pone minas.");
    }
}

public class Artillero extends Soldado {
    public Artillero(String nombre) {
        super(nombre);
    }
    
    @Override
    public void atacar() {
        System.out.println(nombre + " dispara cohetes.");
    }
}

public class Main {
    public static void main(String[] args) {
        // Soldado s = new Soldado("Juan");  // ERROR: clase abstracta
        
        Soldado[] ejercito = new Soldado[2];
        ejercito[0] = new Zapador("María");
        ejercito[1] = new Artillero("Carlos");
        
        for (Soldado s : ejercito) {
            s.atacar();  // Polimorfismo: cada uno ataca diferente
        }
    }
}
```

Una clase abstracta es una clase que no puede ser instanciada directamente y existe para ser heredada. Sirve como plantilla que define qué métodos deben implementar sus subclases. Un método abstracto es un método sin implementación (solo tiene la firma) que obliga a todas las subclases a proporcionarle una implementación. No se puede crear una instancia de una clase abstracta; intentarlo resultaría en un error de compilación. Si una subclase hereda de una clase abstracta sin implementar todos sus métodos abstractos, la subclase también debe declararse como abstracta.

La palabra clave `abstract` debe colocarse en dos lugares: antes de la palabra clave `class` para la clase abstracta, y también antes del tipo de retorno del método abstracto. Esto indica al compilador que esta clase no puede ser instanciada y que sus subclases tienen la responsabilidad de implementar los métodos abstractos.

```java
public abstract class Soldado {
    protected String nombre;
    
    public Soldado(String nombre) {
        this.nombre = nombre;
    }
    
    public void saludar() {
        System.out.println("Soldado " + nombre + " saluda.");
    }
    
    // Método abstracto - sin implementación
    public abstract void atacar();
}

public class Zapador extends Soldado {
    public Zapador(String nombre) {
        super(nombre);
    }
    
    @Override
    public void atacar() {
        System.out.println(nombre + " pone minas.");
    }
}

public class Artillero extends Soldado {
    public Artillero(String nombre) {
        super(nombre);
    }
    
    @Override
    public void atacar() {
        System.out.println(nombre + " dispara cohetes.");
    }
}

public class Main {
    public static void main(String[] args) {
        // Soldado s = new Soldado("Juan");  // ERROR: clase abstracta
        
        Soldado[] ejercito = new Soldado[2];
        ejercito[0] = new Zapador("María");
        ejercito[1] = new Artillero("Carlos");
        
        for (Soldado s : ejercito) {
            s.atacar();  // Polimorfismo: cada uno ataca diferente
        }
    }
}
```


## 8. ¿Qué efecto tiene la palabra clave `final` sobre métodos y clases en Java? ¿Cómo se relaciona con el polimorfismo? ¿Conoces algún ejemplo de clase `final` en la propia API estándar de Java?

### Respuesta

La palabra clave `final` previene que una clase sea heredada o que un método sea sobreescrito. Cuando se marca una clase como `final`, no se puede crear ninguna subclase de ella. Cuando se marca un método como `final`, no se puede sobreescribir en subclases, aunque la clase pueda ser heredada. Esto elimina la posibilidad de polimorfismo para esa clase o método específico.

Los programadores usan `final` cuando desean asegurar que el comportamiento de una clase o método no sea modificado por subclases. Esto puede ser por razones de seguridad (por ejemplo, la clase `String` es `final` para asegurar su inmutabilidad), rendimiento (el compilador puede hacer optimizaciones si sabe que un método no será sobreescrito) o diseño (cuando los desarrolladores consideran que la clase está completa y no debe ser modificada). En la API estándar de Java, ejemplos de clases `final` incluyen `String`, `Integer`, `Double` y otras clases "wrapper" primitivas, así como muchas clases de colecciones inmutables.

```java
public final class Clase1 {  // No puede ser heredada
    public final void metodo() {  // No puede ser sobreescrito
        System.out.println("Implementación final");
    }
}

// ERROR: No se puede heredar de clase final
// public class Subclase extends Clase1 { }
```

La palabra clave `final` previene que una clase sea heredada o que un método sea sobreescrito. Cuando se marca una clase como `final`, no se puede crear ninguna subclase de ella. Cuando se marca un método como `final`, no se puede sobreescribir en subclases, aunque la clase pueda ser heredada. Esto elimina la posibilidad de polimorfismo para esa clase o método específico.

Los programadores usan `final` cuando desean asegurar que el comportamiento de una clase o método no sea modificado por subclases. Esto puede ser por razones de seguridad (por ejemplo, la clase `String` es `final` para asegurar su inmutabilidad), rendimiento (el compilador puede hacer optimizaciones si sabe que un método no será sobreescrito) o diseño (cuando los desarrolladores consideran que la clase está completa y no debe ser modificada). En la API estándar de Java, ejemplos de clases `final` incluyen `String`, `Integer`, `Double` y otras clases "wrapper" primitivas, así como muchas clases de colecciones inmutables.

```java
public final class Clase1 {  // No puede ser heredada
    public final void metodo() {  // No puede ser sobreescrito
        System.out.println("Implementación final");
    }
}

// ERROR: No se puede heredar de clase final
// public class Subclase extends Clase1 { }
```


## 9. En Java, qué son las **"interfaces"**? ¿Son como clases abstractas? ¿Una clase puede implementar más de una interfaz?

### Respuesta

Una interfaz es un contrato que especifica qué métodos debe implementar una clase, pero no cómo implementarlos. Es similar a una clase abstracta pero más restrictiva: todos los métodos de una interfaz son implícitamente abstractos (aunque en Java 8+ pueden tener implementaciones con `default`), y no puede tener atributos (solo constantes). Las interfaces no se heredan sino que se implementan, usando la palabra clave `implements`. Mientras que las clases abstractas pueden tener código común y métodos concretos, las interfaces son principalmente un contrato sobre qué comportamiento se espera.

Sí, una clase puede implementar múltiples interfaces, lo que proporciona una forma de lograr polimorfismo múltiple sin los problemas de la herencia múltiple de clases. Esto es una de las grandes ventajas de las interfaces: una clase puede cumplir múltiples contratos simultáneamente. Por ejemplo, una clase puede implementar tanto `Serializable` como `Comparable` si ambas capacidades tiene sentido para ella. Esto permite flexibilidad en el diseño sin acoplamiento jerárquico.

```java
public interface Atacable {
    void atacar();
    int calcularDaño();
}

public interface Defensa {
    void defenderse();
    int calcularProteccion();
}

public class Soldado implements Atacable, Defensa {
    @Override
    public void atacar() { /* ... */ }
    
    @Override
    public int calcularDaño() { return 0; }
    
    @Override
    public void defenderse() { /* ... */ }
    
    @Override
    public int calcularProteccion() { return 0; }
}
```

Una interfaz es un contrato que especifica qué métodos debe implementar una clase, pero no cómo implementarlos. Es similar a una clase abstracta pero más restrictiva: todos los métodos de una interfaz son implícitamente abstractos (aunque en Java 8+ pueden tener implementaciones con `default`), y no puede tener atributos (solo constantes). Las interfaces no se heredan sino que se implementan, usando la palabra clave `implements`. Mientras que las clases abstractas pueden tener código común y métodos concretos, las interfaces son principalmente un contrato sobre qué comportamiento se espera.

Sí, una clase puede implementar múltiples interfaces, lo que proporciona una forma de lograr polimorfismo múltiple sin los problemas de la herencia múltiple de clases. Esto es una de las grandes ventajas de las interfaces: una clase puede cumplir múltiples contratos simultáneamente. Por ejemplo, una clase puede implementar tanto `Serializable` como `Comparable` si ambas capacidades tiene sentido para ella. Esto permite flexibilidad en el diseño sin acoplamiento jerárquico.

```java
public interface Atacable {
    void atacar();
    int calcularDaño();
}

public interface Defensa {
    void defenderse();
    int calcularProteccion();
}

public class Soldado implements Atacable, Defensa {
    @Override
    public void atacar() { /* ... */ }
    
    @Override
    public int calcularDaño() { return 0; }
    
    @Override
    public void defenderse() { /* ... */ }
    
    @Override
    public int calcularProteccion() { return 0; }
}
```


## 10. Vamos a poner un ejemplo nuevo con polimorfismo. Queremos implementar una clase `Punto`, con un método `calcularDistanciaA`, que permite calcular la distancia a otro `Punto`. Sin embargo, como queremos trabajar con puntos 2D y 3D, haz que ese método sea abstracto y haya dos implementaciones de ese cálculo de distancia. Emplea `instanceof` y *downcasting* para verificar que se recibe un punto compatible y poder calcular correctamente la distancia siempre entre puntos del mismo subtipo. Aprovecha este diseño para crear ahora una clase `Linea`, que acepta `Punto`, sin saber de qué tipo es, y es capaz de dar su longitud independientemente de las dimensiones de sus puntos (las cuales desconoce).

### Respuesta

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
        if (!(otro instanceof Punto2D)) {
            throw new IllegalArgumentException(
                "No se puede calcular distancia entre puntos de diferente dimensión");
        }
        Punto2D p = (Punto2D) otro;
        double dx = this.x - p.x;
        double dy = this.y - p.y;
        return Math.sqrt(dx * dx + dy * dy);
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
        if (!(otro instanceof Punto3D)) {
            throw new IllegalArgumentException(
                "No se puede calcular distancia entre puntos de diferente dimensión");
        }
        Punto3D p = (Punto3D) otro;
        double dx = this.x - p.x;
        double dy = this.y - p.y;
        double dz = this.z - p.z;
        return Math.sqrt(dx * dx + dy * dy + dz * dz);
    }
}

public class Linea {
    private Punto p1, p2;
    
    public Linea(Punto p1, Punto p2) {
        this.p1 = p1;
        this.p2 = p2;
    }
    
    public double getLongitud() {
        // Polimorfismo: no sabe qué tipo de punto es, pero funciona
        return p1.calcularDistanciaA(p2);
    }
}

public class Main {
    public static void main(String[] args) {
        // Línea 2D
        Punto p1 = new Punto2D(0, 0);
        Punto p2 = new Punto2D(3, 4);
        Linea linea2D = new Linea(p1, p2);
        System.out.println("Longitud 2D: " + linea2D.getLongitud());
        
        // Línea 3D
        Punto p3 = new Punto3D(0, 0, 0);
        Punto p4 = new Punto3D(1, 2, 2);
        Linea linea3D = new Linea(p3, p4);
        System.out.println("Longitud 3D: " + linea3D.getLongitud());
    }
}
```

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
        if (!(otro instanceof Punto2D)) {
            throw new IllegalArgumentException(
                "No se puede calcular distancia entre puntos de diferente dimensión");
        }
        Punto2D p = (Punto2D) otro;
        double dx = this.x - p.x;
        double dy = this.y - p.y;
        return Math.sqrt(dx * dx + dy * dy);
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
        if (!(otro instanceof Punto3D)) {
            throw new IllegalArgumentException(
                "No se puede calcular distancia entre puntos de diferente dimensión");
        }
        Punto3D p = (Punto3D) otro;
        double dx = this.x - p.x;
        double dy = this.y - p.y;
        double dz = this.z - p.z;
        return Math.sqrt(dx * dx + dy * dy + dz * dz);
    }
}

public class Linea {
    private Punto p1, p2;
    
    public Linea(Punto p1, Punto p2) {
        this.p1 = p1;
        this.p2 = p2;
    }
    
    public double getLongitud() {
        // Polimorfismo: no sabe qué tipo de punto es, pero funciona
        return p1.calcularDistanciaA(p2);
    }
}

public class Main {
    public static void main(String[] args) {
        // Línea 2D
        Punto p1 = new Punto2D(0, 0);
        Punto p2 = new Punto2D(3, 4);
        Linea linea2D = new Linea(p1, p2);
        System.out.println("Longitud 2D: " + linea2D.getLongitud());
        
        // Línea 3D
        Punto p3 = new Punto3D(0, 0, 0);
        Punto p4 = new Punto3D(1, 2, 2);
        Linea linea3D = new Linea(p3, p4);
        System.out.println("Longitud 3D: " + linea3D.getLongitud());
    }
}
```


## 11. ¿Qué es la **"herencia de interfaces"** en Java? ¿Existe **"herencia múltiple de interfaces"**? Pon un ejemplo de una interfaz `Fichero` que tenga un método para leer su contenido en forma de `String` y luego dicha interfaz sea extendida por otra que sea `FicheroEscribible` que permita enviar contenido e incluso eliminar el fichero.

### Respuesta

La herencia de interfaces ocurre cuando una interfaz extiende otra interfaz, heredando sus métodos. Una interfaz puede extender múltiples interfaces simultáneamente (herencia múltiple de interfaces), cosa que las clases no pueden hacer con herencia de clases. Esto es seguro porque las interfaces no contienen estado (solo constantes) ni implementación conflictiva, evitando los problemas del "diamond problem". Una clase que implementa una interfaz que extiende otra, debe implementar todos los métodos de ambas.

```java
public interface Fichero {
    String leerContenido();
}

public interface FicheroEscribible extends Fichero {
    void escribirContenido(String contenido);
    void eliminar();
}

public class FicheroDeTexto implements FicheroEscribible {
    private String contenido = "";
    
    @Override
    public String leerContenido() {
        return contenido;
    }
    
    @Override
    public void escribirContenido(String contenido) {
        this.contenido = contenido;
    }
    
    @Override
    public void eliminar() {
        this.contenido = "";
        System.out.println("Fichero eliminado");
    }
}

public class Main {
    public static void main(String[] args) {
        FicheroEscribible fichero = new FicheroDeTexto();
        fichero.escribirContenido("Hola mundo");
        System.out.println(fichero.leerContenido());  // Heredado de Fichero
        fichero.eliminar();
    }
}
```

La herencia de interfaces ocurre cuando una interfaz extiende otra interfaz, heredando sus métodos. Una interfaz puede extender múltiples interfaces simultáneamente (herencia múltiple de interfaces), cosa que las clases no pueden hacer con herencia de clases. Esto es seguro porque las interfaces no contienen estado (solo constantes) ni implementación conflictiva, evitando los problemas del "diamond problem". Una clase que implementa una interfaz que extiende otra, debe implementar todos los métodos de ambas.

```java
public interface Fichero {
    String leerContenido();
}

public interface FicheroEscribible extends Fichero {
    void escribirContenido(String contenido);
    void eliminar();
}

public class FicheroDeTexto implements FicheroEscribible {
    private String contenido = "";
    
    @Override
    public String leerContenido() {
        return contenido;
    }
    
    @Override
    public void escribirContenido(String contenido) {
        this.contenido = contenido;
    }
    
    @Override
    public void eliminar() {
        this.contenido = "";
        System.out.println("Fichero eliminado");
    }
}

public class Main {
    public static void main(String[] args) {
        FicheroEscribible fichero = new FicheroDeTexto();
        fichero.escribirContenido("Hola mundo");
        System.out.println(fichero.leerContenido());  // Heredado de Fichero
        fichero.eliminar();
    }
}
```
