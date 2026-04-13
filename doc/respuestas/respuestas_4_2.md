<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Herencia". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación, Excepciones y Composición.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# Tema 4.2. Herencia

## 1. En orientación a objetos, ¿qué es la **herencia** y su relación con "A es-un B"?. Explica las dos implicaciones principales: (1) **compatibilidad de tipos** y (2) **herencia de estado y comportamiento**. Pon un ejemplo en Java muy sencillo, donde un `Soldado` tiene un `nombre` (privado) y un método `saludar()` que muestra su nombre. Hay dos subtipos: un `Artillero`, que es capaz de disparar cohetes y un `Zapador` que pone minas, ambos heredan el atributo nombre y la capacidad de saludar. Además, y de forma específica, el artillero tiene un número de cohetes y el zapador un número de minas, accesibles mediante "getters" específicos. Respecto a la compatibilidad de tipos, aprovechémosla: crea un array de `Soldado`, mete varios de distinto tipo (son todos compatibles con `Soldado`). Recórrela y que todos te saluden.

### Respuesta

La herencia es un mecanismo de la orientación a objetos que permite crear nuevas clases basadas en clases existentes, estableciendo una relación "A es-un B" donde la subclase hereda características de la superclase. La compatibilidad de tipos significa que una referencia de la superclase puede apuntar a objetos de cualquier subclase, permitiendo tratarlos de forma uniforme. La herencia de estado y comportamiento significa que la subclase obtiene automáticamente todos los atributos y métodos de la superclase, ganando así funcionalidad preexistente sin duplicar código.

```java
public class Soldado {
    private String nombre;
    
    public Soldado(String nombre) {
        this.nombre = nombre;
    }
    
    public void saludar() {
        System.out.println("Hola, soy " + nombre);
    }
}

public class Artillero extends Soldado {
    private int numCohetes;
    
    public Artillero(String nombre, int numCohetes) {
        super(nombre);
        this.numCohetes = numCohetes;
    }
    
    public int getNumCohetes() {
        return numCohetes;
    }
    
    public void dispararCohetes() {
        System.out.println("¡Disparando " + numCohetes + " cohetes!");
    }
}

public class Zapador extends Soldado {
    private int numMinas;
    
    public Zapador(String nombre, int numMinas) {
        super(nombre);
        this.numMinas = numMinas;
    }
    
    public int getNumMinas() {
        return numMinas;
    }
    
    public void ponerMinas() {
        System.out.println("Poniendo " + numMinas + " minas.");
    }
}

public class Main {
    public static void main(String[] args) {
        // Array de Soldado (superclase)
        Soldado[] ejercito = new Soldado[3];
        
        // Agregar objetos de diferentes subclases (compatibilidad de tipos)
        ejercito[0] = new Artillero("Juan", 5);
        ejercito[1] = new Zapador("María", 10);
        ejercito[2] = new Artillero("Carlos", 7);
        
        // Recorrer y hacer que todos saluden (herencia de comportamiento)
        for (Soldado s : ejercito) {
            s.saludar();
        }
    }
}
```

## 2. Al crear los soldados concretos, ¿cuántos constructores se ejecutan y en qué orden? ¿Qué significa `super` dentro de un constructor? Si la clase base no tiene visible el constructor sin parámetros, ¿debo llamar a `super` siempre? 

### Respuesta

Cuando se crea un objeto de una subclase, se ejecutan múltiples constructores en orden jerárquico: primero el constructor de la superclase y luego el de la subclase. En Java, este proceso ocurre de forma automática o explícita mediante la palabra clave `super`. La palabra clave `super` dentro de un constructor es una llamada explícita al constructor de la superclase, permitiendo pasar los argumentos necesarios para inicializar el estado heredado de la clase padre.

Si la clase base no tiene visible un constructor sin parámetros (constructores privados o solo constructores con parámetros), entonces sí es obligatorio llamar explícitamente a `super` con los parámetros correspondientes en el constructor de la subclase. Si la superclase tiene un constructor público sin parámetros, Java lo llamará automáticamente si no se especifica una llamada `super` explícita. Sin embargo, es una buena práctica siempre llamar a `super` explícitamente para hacer el código más legible y mantener claridad sobre qué se está inicializando.

```java
public class Artillero extends Soldado {
    private int numCohetes;
    
    public Artillero(String nombre, int numCohetes) {
        super(nombre);  // Llama al constructor de Soldado
        this.numCohetes = numCohetes;
    }
}
```

## 3. Respecto a los objetos de subclases en memoria, los atributos privados de la superclase, ¿forman parte de una instancia de la subclase en memoria? En caso afirmativo ¿implica que se puedan usar desde el código de la subclase? Explícalo con el ejemplo de `Soldado` y alguna de sus subclases.

### Respuesta

Los atributos privados de la superclase sí forman parte de una instancia de la subclase en memoria. Ambas partes, la heredada y la propia, están presentes en el objeto cuando se crea una instancia de la subclase. Sin embargo, la encapsulación se mantiene: aunque los atributos privados existen en memoria como parte del objeto subclase, no pueden ser accedidos directamente desde el código de la subclase debido al modificador `private`, que restringe el acceso solo a la clase que los define.

En el ejemplo de `Soldado`, el atributo privado `nombre` forma parte de cualquier instancia de `Artillero` o `Zapador` en memoria, pero el código de estas subclases no puede acceder directamente a `nombre`. Solo pueden acceder a través de métodos públicos de la superclase como `saludar()`. Esta es una característica deliberada que refuerza la encapsulación: la superclase controla cómo se utilizan sus atributos privados, incluso desde las subclases.

```java
public class Zapador extends Soldado {
    private int numMinas;
    
    public void ponerMinas() {
        // System.out.println(nombre);  // ERROR: 'nombre' es privado
        saludar();  // OK: Métodos públicos de la superclase sí son accesibles
        System.out.println("Poniendo " + numMinas + " minas.");
    }
}
```

## 4. ¿Qué implica en términos de **extensibilidad** de código el hecho de que sean compatibles a nivel de tipos? Ilustra esto añadiendo un nuevo tipo de `Soldado` y demostrando que el código para pedir el saludo a todos los soldados no se modifica.

### Respuesta

La compatibilidad de tipos en la herencia proporciona una extensibilidad extraordinaria: el código que trabaja con la superclase puede trabajar con cualquier subclase sin necesidad de modificación. Esto permite agregar nuevas funcionalidades y tipos al sistema sin cambiar el código existente que ya funciona. Esta es una aplicación del principio abierto/cerrado de la ingeniería de software: el código está abierto para extensión (agregar nuevas subclases) pero cerrado para modificación (el código cliente no cambia).

```java
// Nueva subclase sin modificar código existente
public class Medico extends Soldado {
    private int botiquines;
    
    public Medico(String nombre, int botiquines) {
        super(nombre);
        this.botiquines = botiquines;
    }
    
    public void curar() {
        System.out.println("Curando con " + botiquines + " botiquines.");
    }
}

public class Main {
    public static void main(String[] args) {
        Soldado[] ejercito = new Soldado[4];
        ejercito[0] = new Artillero("Juan", 5);
        ejercito[1] = new Zapador("María", 10);
        ejercito[2] = new Artillero("Carlos", 7);
        ejercito[3] = new Medico("Ana", 3);  // Nuevo tipo
        
        // El mismo código FUNCIONA SIN CAMBIOS
        for (Soldado s : ejercito) {
            s.saludar();
        }
    }
}
```

## 5. En Java, cuando trabajo con referencias y herencia. ¿Puedo tener una referencia del supertipo que apunte a objetos reales de un subtipo? ¿Puedo invocar con la referencia del supertipo a métodos públicos del subtipo? ¿En qué consiste el **"upcasting"** y el **"downcasting"**? ¿Qué es el `instanceof`? Pon un ejemplo de recorrido de un array de `Soldado`, comprobando que, si el objeto real es un `Artillero`, solicite el número de cohetes que tiene y los imprima.

### Respuesta

Sí es posible tener una referencia del supertipo que apunte a objetos reales de un subtipo; esto es automático en Java. Sin embargo, con la referencia del supertipo solo se pueden invocar métodos que estén definidos en la superclase. Para acceder a métodos específicos del subtipo, se debe convertir la referencia mediante "downcasting". El upcasting es la conversión implícita de una referencia de subtipo a supertipo (siempre segura), mientras que el downcasting es la conversión explícita de una referencia de supertipo a subtipo (potencialmente insegura). El operador `instanceof` verifica si un objeto es una instancia de una clase específica, permitiendo hacer downcasting de forma segura antes de usar métodos específicos de la subclase.

```java
public class Main {
    public static void main(String[] args) {
        Soldado[] ejercito = new Soldado[3];
        ejercito[0] = new Artillero("Juan", 5);
        ejercito[1] = new Zapador("María", 10);
        ejercito[2] = new Artillero("Carlos", 7);
        
        for (Soldado s : ejercito) {
            s.saludar();
            
            // Downcasting seguro con instanceof
            if (s instanceof Artillero) {
                Artillero art = (Artillero) s;  // Downcasting explícito
                System.out.println("  Número de cohetes: " + art.getNumCohetes());
            } else if (s instanceof Zapador) {
                Zapador zap = (Zapador) s;  // Downcasting explícito
                System.out.println("  Número de minas: " + zap.getNumMinas());
            }
        }
    }
}
```

## 6. Respecto a la ocultación de información y herencia, ¿qué significa acceso **"protegido"** de métodos y/o atributos? ¿Cómo se implementa en Java? Pon un ejemplo de uso de en la clase `Soldado` para que su nombre sea protegido y pueda usarse en el método de poner bombas del `Zapador`.

### Respuesta

El acceso protegido es un nivel de visibilidad intermedio entre privado y público que permite que los atributos y métodos sean accesibles dentro de la clase que los define y en cualquier subclase, pero no públicamente desde fuera. En Java se implementa usando el modificador `protected`. Esto es especialmente útil en herencia porque permite que la superclase comparta información con sus subclases de forma controlada, manteniendo la encapsulación ante el resto del mundo. Con acceso protegido, las subclases pueden colaborar con la superclase de forma más estrecha sin necesidad de métodos públicos.

```java
public class Soldado {
    protected String nombre;  // Acceso protegido
    
    public Soldado(String nombre) {
        this.nombre = nombre;
    }
    
    public void saludar() {
        System.out.println("Hola, soy " + nombre);
    }
}

public class Zapador extends Soldado {
    private int numMinas;
    
    public Zapador(String nombre, int numMinas) {
        super(nombre);
        this.numMinas = numMinas;
    }
    
    public void ponerMinas() {
        // Ahora puede acceder 'nombre' protegido de la superclase
        System.out.println(nombre + " está poniendo " + numMinas + " minas.");
    }
}
```

## 7. En los lenguajes orientados a objetos ¿hay una **clase base** para todos los objetos? ¿Ocurre en todos los lenguajes? ¿Qué ocurre en Java?

### Respuesta

No todos los lenguajes orientados a objetos tienen una clase base única para todos los objetos. Algunos lenguajes, como Smalltalk y Ruby, tienen una jerarquía única donde todas las clases heredan de una clase raíz común. Otros lenguajes, como C++, permite crear jerarquías múltiples e independientes sin una clase base común. Java, sin embargo, tiene una clase base universal llamada `Object`. Todas las clases en Java, incluso las que el programador crea sin especificar explícitamente una superclase, heredan implícitamente de `Object`.

Esta clase `Object` proporciona métodos fundamentales a todos los objetos Java, como `toString()`, `equals()`, `hashCode()` y `getClass()`. Hereda implícitamente de `Object` incluso si no se escribe `extends Object` explícitamente. Esta jerarquía única da a Java una consistencia y uniformidad que facilita trabajar con objetos de forma polimórfica.

## 8. ¿Qué es la **"herencia múltiple"**? ¿Existe en Java herencia múltiple?

### Respuesta

La herencia múltiple es la capacidad de una clase de heredar de más de una superclase simultáneamente. Aunque conceptualmente poderosa, introduce complejidades significativas. Lenguajes como C++ permiten herencia múltiple, pero esto lleva a problemas como el "diamond problem" (problema del diamante), donde si dos superclases heredan de una clase común, la subclase hereda dos copias de esa clase, causando ambigüedad y redundancia. Por estas razones de complejidad, Java rechazó la herencia múltiple en favor de un modelo más simple.

Java no soporta herencia múltiple de clases, es decir, una clase no puede extender más de una clase simultáneamente. En su lugar, Java proporciona interfaces, que son contratos que permiten implementar comportamientos de múltiples fuentes de forma segura. Una clase puede implementar múltiples interfaces, logrando así proporcionar múltiples comportamientos sin los problemas de la herencia múltiple de clases.

## 9. Las excepciones en los lenguajes orientados a objetos son objetos. Por tanto, se pueden crear excepciones personalizadas. Pon un ejemplo en Java de una excepción personalizada (`UsuarioNoEncontradoException`), que sea *no controlada* y que además este compuesto con un `Usuario`, para saber qué `Usuario` dio el problema. Permite además que se pueda incluir la causa, es decir, sobrecarga el constructor para tener una versión que permita añadir la causa subyacente. 

### Respuesta

Las excepciones en Java son objetos que heredan de `Throwable`. Existen excepciones controladas (que deben capturarse) e incontroladas (que heredan de `RuntimeException`). Para crear una excepción personalizada no controlada, se hereda de `RuntimeException`. Se puede usar composición para incluir información adicional como el `Usuario` que causó el error. Además, Java permite encadenar excepciones mediante la causa, permitiendo preservar la información del error original mientras se lanza una excepción de más alto nivel.

```java
public class Usuario {
    private String id;
    private String nombre;
    
    public Usuario(String id, String nombre) {
        this.id = id;
        this.nombre = nombre;
    }
    
    public String getId() { return id; }
    public String getNombre() { return nombre; }
    
    @Override
    public String toString() {
        return "Usuario{" + "id='" + id + '\'' + 
               ", nombre='" + nombre + '\'' + '}';
    }
}

public class UsuarioNoEncontradoException extends RuntimeException {
    private Usuario usuario;
    
    // Constructor sin causa
    public UsuarioNoEncontradoException(String mensaje, Usuario usuario) {
        super(mensaje);
        this.usuario = usuario;
    }
    
    // Constructor sobrecargado con causa
    public UsuarioNoEncontradoException(String mensaje, Usuario usuario, 
                                        Throwable causa) {
        super(mensaje, causa);
        this.usuario = usuario;
    }
    
    public Usuario getUsuario() {
        return usuario;
    }
}

public class Main {
    public static void main(String[] args) {
        Usuario usuarioProblematico = new Usuario("123", "Juan");
        
        try {
            throw new UsuarioNoEncontradoException(
                "El usuario no se encontró en la base de datos",
                usuarioProblematico,
                new Exception("Connection timeout"));
        } catch (UsuarioNoEncontradoException e) {
            System.out.println("Error: " + e.getMessage());
            System.out.println("Usuario problemático: " + e.getUsuario());
            System.out.println("Causa raíz: " + e.getCause());
        }
    }
}
```

## 10. Herencia vs. Composición. Se dice que no se debe emplear herencia simplemente por reutilizar código, es decir, que si quiero reutilizar código simplemente, no debo pensar en herencia como primera opción ¿por qué?

### Respuesta

La herencia establece una jerarquía y una relación fuerte de "es-un". Cuando se usa herencia únicamente para reutilizar código sin que la relación "es-un" sea semánticamente correcta, se introduce acoplamiento innecesario y rigidez en el diseño. Por ejemplo, si una clase `Coche` hereda de `Motor` solo para reutilizar el código de aceleración, esto es conceptualmente incorrecto; un coche no "es-un" motor, sino que "tiene-un" motor.

Usar herencia de forma inapropiada hace que cambios en la superclase afecten a todas las subclases potencialmente, limitando la flexibilidad futura. La composición es más flexible: permite cambiar el comportamiento en tiempo de ejecución y evita el acoplamiento jerárquico. Si se necesita reutilizar código, generalmente es mejor usar composición (delegar a un objeto que contiene el código reutilizable) que herencia, reservando la herencia para casos donde la relación "es-un" es verdaderamente apropiada.

## 11. Herencia vs. Composición. Se dice que se debe *"favorecer la composición frente a la herencia"*, ¿por qué?

### Respuesta

La composición ofrece mayor flexibilidad y menores riesgos que la herencia. Con composición, un objeto contiene referencias a otros objetos y les delega responsabilidades, lo que permite cambiar el comportamiento en tiempo de ejecución reemplazando los objetos componentes sin necesidad de modificar la estructura de clases. Con herencia, el comportamiento está vinculado en tiempo de compilación y modificar una superclase puede afectar inesperadamente a todas sus subclases.

Además, la composición evita el problema del acoplamiento fuerte: una clase que usa composición es menos dependiente de la implementación de sus componentes que una subclase de su superclase. La composición también es más fácil de entender y mantener porque las relaciones son explícitas: si una clase B está compuesta de un objeto A, eso es completamente obvio en el código. La recomendación general es usar herencia solo cuando la relación "es-un" es clara y natural, y favorecer composición el resto del tiempo.

## 12. Herencia vs. Composición. Se dice que la *"herencia rompe la encapsulación"*, ¿a qué se refiere esto?

### Respuesta

La herencia rompe la encapsulación porque las subclases necesitan conocer detalles internos de la superclase para extenderla correctamente. Los miembros protegidos de la superclase se exponen intencionalmente a las subclases, violando el principio de ocultación de información. Cambios en la implementación de la superclase, incluso cambios internos que no afectan su interfaz pública, pueden romper las subclases si dependen de esos detalles internos. Por ejemplo, si una superclase cambia cómo almacena un atributo protegido, todas las subclases que acceden a ese atributo pueden verse afectadas.

Con composición, en cambio, la encapsulación se preserva completamente. Una clase que usa composición solo interactúa con la interfaz pública del objeto que contiene, no con sus detalles internos. Si la implementación interna del objeto contenido cambia, la clase que lo contiene no se ve afectada siempre que la interfaz pública permanezca igual. Por esta razón, la composición es más robusta frente a cambios en el código y mantiene mejor la separación de responsabilidades.

## 13. Pongamos un ejemplo de dos alternativas para lo mismo. Tenemos un `Estudiante` y un `Trabajador`, ambos tienen datos en común: el DNI y el nombre. Modelemos esto de dos formas: uno por herencia, con una superclase `Persona`, y otro con composición, con una clase `DatosPersonales`. Se debe recibir una instancia de `DatosPersonales` en el constructor de la clase `Estudiante` y `Trabajador`.

### Respuesta

Este ejemplo ilustra claramente cómo herencia y composición resuelven el mismo problema de maneras diferentes. Con herencia, se crea una clase base `Persona` con los atributos comunes (DNI y nombre), y `Estudiante` y `Trabajador` heredan de ella. Con composición, se crea una clase `DatosPersonales` que encapsula esos datos comunes, y tanto `Estudiante` como `Trabajador` tienen una instancia de `DatosPersonales` como componente. La composición es más flexible aquí porque permite que `Estudiante` y `Trabajador` evolucionen independientemente sin estar vinculados por una jerarquía de herencia.

**Solución con Herencia:**

```java
public class Persona {
    protected String dni;
    protected String nombre;
    
    public Persona(String dni, String nombre) {
        this.dni = dni;
        this.nombre = nombre;
    }
    
    public String getDni() { return dni; }
    public String getNombre() { return nombre; }
}

public class Estudiante extends Persona {
    private String matricula;
    
    public Estudiante(String dni, String nombre, String matricula) {
        super(dni, nombre);
        this.matricula = matricula;
    }
    
    public String getMatricula() { return matricula; }
}

public class Trabajador extends Persona {
    private String numeroEmpleado;
    
    public Trabajador(String dni, String nombre, String numeroEmpleado) {
        super(dni, nombre);
        this.numeroEmpleado = numeroEmpleado;
    }
    
    public String getNumeroEmpleado() { return numeroEmpleado; }
}
```

**Solución con Composición:**

```java
public class DatosPersonales {
    private String dni;
    private String nombre;
    
    public DatosPersonales(String dni, String nombre) {
        this.dni = dni;
        this.nombre = nombre;
    }
    
    public String getDni() { return dni; }
    public String getNombre() { return nombre; }
}

public class Estudiante {
    private DatosPersonales datos;
    private String matricula;
    
    public Estudiante(DatosPersonales datos, String matricula) {
        this.datos = datos;
        this.matricula = matricula;
    }
    
    public DatosPersonales getDatos() { return datos; }
    public String getMatricula() { return matricula; }
}

public class Trabajador {
    private DatosPersonales datos;
    private String numeroEmpleado;
    
    public Trabajador(DatosPersonales datos, String numeroEmpleado) {
        this.datos = datos;
        this.numeroEmpleado = numeroEmpleado;
    }
    
    public DatosPersonales getDatos() { return datos; }
    public String getNumeroEmpleado() { return numeroEmpleado; }
}

public class Main {
    public static void main(String[] args) {
        DatosPersonales datos = new DatosPersonales("12345678A", "Juan");
        Estudiante estudiante = new Estudiante(datos, "E001");
        Trabajador trabajador = new Trabajador(datos, "T001");
    }
}
```
