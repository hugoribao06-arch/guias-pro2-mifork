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

La herencia en orientación a objetos es un mecanismo que permite definir una clase nueva a partir de otra ya existente, reutilizando sus características. Se basa en la relación “A es-un B”, lo que implica que una instancia de la subclase también puede considerarse una instancia de la superclase. De este modo, si se afirma que un artillero es un soldado, cualquier objeto de tipo artillero puede tratarse como si fuera un soldado, ya que comparte su naturaleza básica.

Una de las implicaciones más importantes es la compatibilidad de tipos. Esto significa que los objetos de una subclase pueden utilizarse allí donde se espera un objeto de la superclase. Gracias a esto, es posible trabajar con estructuras generales que contienen distintos tipos concretos sin necesidad de diferenciarlos explícitamente. Por ejemplo, se puede definir un array de soldados y almacenar en él tanto artilleros como zapadores, ya que ambos son tipos compatibles con soldado. Esto facilita escribir código más general y flexible.

La otra implicación clave es la herencia de estado y comportamiento. La subclase hereda los atributos y métodos de la superclase, evitando duplicar código y permitiendo reutilizar funcionalidades ya implementadas. En el ejemplo propuesto, tanto artillero como zapador heredan el atributo nombre y el método saludar, definidos en la clase soldado. Además, cada subclase puede añadir su propio estado específico, como el número de cohetes o de minas, junto con sus métodos correspondientes, ampliando así el comportamiento base.

class Soldado {
    private String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Hola, soy " + nombre);
    }
}

class Artillero extends Soldado {
    private int numCohetes;

    public Artillero(String nombre, int numCohetes) {
        super(nombre);
        this.numCohetes = numCohetes;
    }

    public int getNumCohetes() {
        return numCohetes;
    }
}

class Zapador extends Soldado {
    private int numMinas;

    public Zapador(String nombre, int numMinas) {
        super(nombre);
        this.numMinas = numMinas;
    }

    public int getNumMinas() {
        return numMinas;
    }
}

public class Main {
    public static void main(String[] args) {
        Soldado[] ejercito = new Soldado[3];

        ejercito[0] = new Artillero("Juan", 5);
        ejercito[1] = new Zapador("Luis", 3);
        ejercito[2] = new Artillero("Carlos", 2);

        for (Soldado s : ejercito) {
            s.saludar();
        }
    }
}

## 2. Al crear los soldados concretos, ¿cuántos constructores se ejecutan y en qué orden? ¿Qué significa `super` dentro de un constructor? Si la clase base no tiene visible el constructor sin parámetros, ¿debo llamar a `super` siempre? 

Al crear un objeto de una subclase, no se ejecuta únicamente el constructor de dicha subclase, sino también el de su superclase. En realidad, se ejecuta una cadena de constructores que comienza en la clase más general y continúa hasta la clase más específica. En el ejemplo, al crear un objeto de tipo Artillero o Zapador, se ejecutan dos constructores: primero el de Soldado y después el de la subclase correspondiente. Este orden es obligatorio, ya que antes de inicializar la parte específica del objeto, debe inicializarse la parte heredada.

La palabra clave super dentro de un constructor se utiliza para invocar explícitamente el constructor de la clase base. Permite pasar los parámetros necesarios para inicializar los atributos heredados. Cuando se escribe super(nombre), se está indicando que el constructor de la superclase debe ejecutarse con ese argumento, asegurando que el estado definido en la clase base quede correctamente inicializado antes de continuar con la subclase.

Si la clase base no dispone de un constructor sin parámetros accesible, es obligatorio llamar a super de forma explícita desde el constructor de la subclase. En Java, si no se escribe ninguna llamada a super, el compilador intenta insertar automáticamente una llamada al constructor sin parámetros de la superclase. Si este no existe o no es visible, se produce un error de compilación. Por tanto, en esos casos, siempre debe indicarse explícitamente qué constructor de la clase base se desea invocar.

## 3. Respecto a los objetos de subclases en memoria, los atributos privados de la superclase, ¿forman parte de una instancia de la subclase en memoria? En caso afirmativo ¿implica que se puedan usar desde el código de la subclase? Explícalo con el ejemplo de `Soldado` y alguna de sus subclases.

Los atributos privados de la superclase sí forman parte de una instancia de la subclase en memoria. Cuando se crea un objeto de una subclase, dicho objeto incluye tanto los atributos definidos en la propia subclase como los heredados de la superclase. Por tanto, un objeto de tipo Artillero contiene internamente el atributo nombre definido en Soldado, aunque no se vea directamente en la definición de la subclase.

Sin embargo, el hecho de que esos atributos existan en memoria no implica que puedan utilizarse directamente desde el código de la subclase. El modificador private restringe el acceso únicamente a la clase donde se declara, por lo que la subclase no puede acceder directamente a esos atributos heredados. Esto significa que, aunque el nombre forme parte del objeto Artillero, no se puede referenciar directamente desde esa clase.

En el ejemplo, un Artillero hereda el atributo nombre de Soldado, pero no puede acceder a él directamente porque es privado. Si se necesitara utilizar ese valor desde la subclase, habría que hacerlo a través de métodos proporcionados por la superclase, como un getter público o protegido. De este modo, se respeta la encapsulación, ya que el acceso a los datos internos sigue estando controlado por la clase que los define.

## 4. ¿Qué implica en términos de **extensibilidad** de código el hecho de que sean compatibles a nivel de tipos? Ilustra esto añadiendo un nuevo tipo de `Soldado` y demostrando que el código para pedir el saludo a todos los soldados no se modifica.

La compatibilidad de tipos que introduce la herencia tiene una consecuencia directa en la extensibilidad del código. Permite que nuevas clases se incorporen al sistema sin necesidad de modificar el código existente que trabaja con el tipo general. Es decir, el comportamiento común se define en la superclase, y cualquier nueva subclase que respete ese contrato puede integrarse automáticamente en las estructuras ya existentes. Esto reduce el acoplamiento y facilita la evolución del programa.

En términos prácticos, esto significa que el código que opera con el tipo base no necesita conocer los detalles de las subclases. Si se añade un nuevo tipo de soldado, no es necesario modificar el bucle que recorre el array ni la forma en que se invoca el método saludar. Mientras la nueva clase herede de Soldado, será compatible y podrá utilizarse exactamente igual que las anteriores. Esto es una de las bases del diseño orientado a objetos, ya que permite ampliar el sistema sin romper lo ya implementado.

A continuación se añade un nuevo tipo de soldado, por ejemplo un Medico, que también hereda de Soldado y tiene su propio atributo específico. El código que recorre el array de soldados no cambia en absoluto:

class Medico extends Soldado {
    private int numBotiquines;

    public Medico(String nombre, int numBotiquines) {
        super(nombre);
        this.numBotiquines = numBotiquines;
    }

    public int getNumBotiquines() {
        return numBotiquines;
    }
}

public class Main {
    public static void main(String[] args) {
        Soldado[] ejercito = new Soldado[4];

        ejercito[0] = new Artillero("Juan", 5);
        ejercito[1] = new Zapador("Luis", 3);
        ejercito[2] = new Artillero("Carlos", 2);
        ejercito[3] = new Medico("Ana", 4);

        for (Soldado s : ejercito) {
            s.saludar();
        }
    }
}

## 5. En Java, cuando trabajo con referencias y herencia. ¿Puedo tener una referencia del supertipo que apunte a objetos reales de un subtipo? ¿Puedo invocar con la referencia del supertipo a métodos públicos del subtipo? ¿En qué consiste el **"upcasting"** y el **"downcasting"**? ¿Qué es el `instanceof`? Pon un ejemplo de recorrido de un array de `Soldado`, comprobando que, si el objeto real es un `Artillero`, solicite el número de cohetes que tiene y los imprima.

Sí puede existir una referencia del supertipo que apunte a un objeto real de un subtipo. De hecho, esa es una de las consecuencias más importantes de la herencia y de la compatibilidad de tipos. Si Artillero y Zapador heredan de Soldado, entonces una variable de tipo Soldado puede almacenar referencias a objetos reales de cualquiera de esas subclases. Esto permite tratar de forma uniforme objetos distintos, siempre que se trabaje con aquello que comparten a través del tipo base.

Sin embargo, con una referencia del supertipo no se pueden invocar directamente métodos que solo existan en el subtipo. El compilador solo permite usar los métodos que pertenezcan al tipo declarado de la referencia. Por tanto, si una variable está declarada como Soldado, se podrán invocar los métodos públicos de Soldado, aunque el objeto real sea un Artillero. Para acceder a métodos específicos de Artillero, antes debe comprobarse que el objeto real pertenece a ese tipo y, después, convertir la referencia al subtipo correspondiente.

El upcasting consiste en tratar un objeto de un subtipo como si fuese de un supertipo. Es una conversión segura y normalmente implícita, porque todo Artillero es un Soldado. El downcasting es la operación inversa: tomar una referencia de tipo general y convertirla en una referencia de un tipo más específico. Esta conversión no siempre es segura, porque no todo Soldado es necesariamente un Artillero. Para evitar errores, se utiliza instanceof, que permite comprobar en tiempo de ejecución si el objeto real pertenece a un tipo concreto antes de realizar el downcasting.

A continuación se muestra un ejemplo en el que se recorre un array de Soldado y, si el objeto real es un Artillero, se obtiene su número de cohetes y se imprime:

class Soldado {
    private String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Hola, soy " + nombre);
    }
}

class Artillero extends Soldado {
    private int numCohetes;

    public Artillero(String nombre, int numCohetes) {
        super(nombre);
        this.numCohetes = numCohetes;
    }

    public int getNumCohetes() {
        return numCohetes;
    }
}

class Zapador extends Soldado {
    private int numMinas;

    public Zapador(String nombre, int numMinas) {
        super(nombre);
        this.numMinas = numMinas;
    }

    public int getNumMinas() {
        return numMinas;
    }
}

public class Main {
    public static void main(String[] args) {
        Soldado[] ejercito = new Soldado[4];
        ejercito[0] = new Artillero("Juan", 5);
        ejercito[1] = new Zapador("Luis", 3);
        ejercito[2] = new Artillero("Carlos", 2);
        ejercito[3] = new Zapador("Pedro", 4);

        for (Soldado s : ejercito) {
            s.saludar();

            if (s instanceof Artillero) {
                Artillero a = (Artillero) s;
                System.out.println("Tiene " + a.getNumCohetes() + " cohetes.");
            }
        }
    }
}

## 6. Respecto a la ocultación de información y herencia, ¿qué significa acceso **"protegido"** de métodos y/o atributos? ¿Cómo se implementa en Java? Pon un ejemplo de uso de en la clase `Soldado` para que su nombre sea protegido y pueda usarse en el método de poner bombas del `Zapador`.

El acceso protegido es un nivel de visibilidad que permite que un atributo o método sea accesible no solo dentro de la propia clase, sino también desde sus subclases. Esto introduce un equilibrio entre la encapsulación estricta (private) y la visibilidad total (public), ya que permite que las clases derivadas reutilicen directamente ciertos elementos internos sin exponerlos completamente al resto del programa. Es especialmente útil cuando se desea que las subclases amplíen o especialicen el comportamiento de la clase base.

En Java, el acceso protegido se implementa mediante la palabra clave protected. Un atributo o método declarado como protegido puede ser utilizado dentro de la clase donde se define y también en cualquier subclase, incluso si está en otro paquete. Sin embargo, sigue sin ser accesible desde clases externas que no formen parte de la jerarquía de herencia, lo que mantiene un cierto grado de ocultación de la información.

En el caso de la clase Soldado, si el atributo nombre se declara como protegido, las subclases como Zapador podrán acceder directamente a él. Esto permite, por ejemplo, que el método específico de Zapador que representa la acción de poner minas pueda utilizar el nombre del soldado sin necesidad de recurrir a un getter. De esta forma, se facilita la reutilización interna entre clases relacionadas sin romper completamente la encapsulación.

class Soldado {
    protected String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Hola, soy " + nombre);
    }
}

class Zapador extends Soldado {
    private int numMinas;

    public Zapador(String nombre, int numMinas) {
        super(nombre);
        this.numMinas = numMinas;
    }

    public void ponerMinas() {
        System.out.println(nombre + " está colocando minas.");
    }

    public int getNumMinas() {
        return numMinas;
    }
}

## 7. En los lenguajes orientados a objetos ¿hay una **clase base** para todos los objetos? ¿Ocurre en todos los lenguajes? ¿Qué ocurre en Java?

En muchos lenguajes orientados a objetos existe el concepto de una clase base común para todos los objetos, aunque no siempre se define de la misma manera ni con el mismo nivel de obligatoriedad. La idea general es que todas las clases compartan un conjunto mínimo de comportamientos básicos, lo que permite tratarlas de forma uniforme en ciertos contextos. Sin embargo, no todos los lenguajes lo implementan de forma estricta; algunos permiten tipos que no derivan de una clase común o manejan este concepto de forma más flexible.

En Java, sí existe una clase base universal llamada Object. Todas las clases heredan directa o indirectamente de esta clase, incluso si no se indica explícitamente en el código. Esto significa que cualquier objeto en Java puede ser tratado como un Object, lo que proporciona un nivel común de comportamiento y permite trabajar con referencias genéricas cuando no se necesita conocer el tipo concreto del objeto.

La clase Object define métodos fundamentales que están disponibles en todos los objetos, como toString, equals o hashCode. Gracias a esto, cualquier clase puede redefinir estos métodos para adaptar su comportamiento, pero siempre existe una implementación básica común. Este diseño facilita la integración de todos los objetos dentro del sistema de tipos de Java y permite aplicar principios de reutilización y generalización en todo el lenguaje.

## 8. ¿Qué es la **"herencia múltiple"**? ¿Existe en Java herencia múltiple?

La herencia múltiple es un mecanismo mediante el cual una clase puede heredar directamente de más de una clase base. Esto permitiría que una clase combine el estado y el comportamiento de varias superclases distintas. En teoría, resulta útil para modelar situaciones donde un objeto pertenece simultáneamente a varias jerarquías, pero también introduce complejidad, especialmente cuando existen conflictos entre métodos o atributos heredados de distintas clases.

Uno de los problemas más conocidos de la herencia múltiple es la ambigüedad que puede surgir cuando dos superclases definen un mismo método o atributo. En ese caso, no queda claro cuál de las implementaciones debe utilizar la subclase. Este tipo de situaciones complica tanto el diseño como el mantenimiento del código, y es una de las razones por las que muchos lenguajes han optado por limitar o evitar este mecanismo.

En Java no existe herencia múltiple de clases. Una clase solo puede extender de una única superclase. Sin embargo, Java permite implementar múltiples interfaces, lo que proporciona una forma controlada de herencia múltiple de comportamiento sin los problemas asociados a la herencia múltiple tradicional. De este modo, se mantiene la flexibilidad sin introducir ambigüedades en la jerarquía de clases.

## 9. Las excepciones en los lenguajes orientados a objetos son objetos. Por tanto, se pueden crear excepciones personalizadas. Pon un ejemplo en Java de una excepción personalizada (`UsuarioNoEncontradoException`), que sea *no controlada* y que además este compuesto con un `Usuario`, para saber qué `Usuario` dio el problema. Permite además que se pueda incluir la causa, es decir, sobrecarga el constructor para tener una versión que permita añadir la causa subyacente. 

En los lenguajes orientados a objetos, las excepciones son objetos que forman parte de una jerarquía de clases. Esto permite definir nuevas excepciones adaptadas a las necesidades de una aplicación concreta. Una excepción personalizada permite representar de forma más precisa un tipo de error específico, en lugar de utilizar excepciones genéricas. Además, al ser objetos, pueden encapsular información adicional relevante sobre el error ocurrido.

En Java, una excepción no controlada es aquella que hereda de RuntimeException. Estas excepciones no están obligadas a ser declaradas ni capturadas, ya que representan errores que normalmente no se espera que el programa recupere de forma directa. Para crear una excepción personalizada no controlada, basta con extender RuntimeException y definir los constructores adecuados.

En este caso, la excepción UsuarioNoEncontradoException incluye un objeto Usuario como parte de su estado, lo que permite conocer qué usuario ha provocado el error. Además, se sobrecargan los constructores para permitir tanto la creación de la excepción con un mensaje simple como con una causa subyacente, lo que facilita encadenar excepciones y mantener información completa sobre el origen del problema.

class Usuario {
    private final String nombre;

    public Usuario(String nombre) {
        if (nombre == null || nombre.isEmpty()) {
            throw new IllegalArgumentException("Nombre inválido");
        }
        this.nombre = nombre;
    }

    public String getNombre() {
        return nombre;
    }
}

class UsuarioNoEncontradoException extends RuntimeException {
    private final Usuario usuario;

    public UsuarioNoEncontradoException(Usuario usuario) {
        super("Usuario no encontrado: " + usuario.getNombre());
        this.usuario = usuario;
    }

    public UsuarioNoEncontradoException(Usuario usuario, Throwable causa) {
        super("Usuario no encontrado: " + usuario.getNombre(), causa);
        this.usuario = usuario;
    }

    public Usuario getUsuario() {
        return usuario;
    }
}

## 10. Herencia vs. Composición. Se dice que no se debe emplear herencia simplemente por reutilizar código, es decir, que si quiero reutilizar código simplemente, no debo pensar en herencia como primera opción ¿por qué?

La herencia no debe utilizarse simplemente como un mecanismo de reutilización de código porque introduce una relación semántica fuerte entre clases. Cuando una clase hereda de otra, se está afirmando que existe una relación del tipo “A es-un B”, lo cual implica un compromiso conceptual, no solo técnico. Si esa relación no es realmente correcta desde el punto de vista del dominio del problema, el diseño se vuelve incorrecto y difícil de mantener, aunque inicialmente permita reutilizar código.

Además, la herencia genera un alto acoplamiento entre la superclase y la subclase. La subclase depende fuertemente de la implementación de la clase base, por lo que cualquier cambio en esta puede afectar al comportamiento de las clases derivadas. Esto dificulta la evolución del sistema, ya que pequeñas modificaciones en la superclase pueden provocar efectos inesperados en toda la jerarquía. Por tanto, usar herencia solo por evitar duplicación de código puede llevar a diseños rígidos y frágiles.

La composición, en cambio, ofrece una alternativa más flexible. Permite reutilizar funcionalidad sin establecer una relación de tipo jerárquica, sino mediante la inclusión de objetos como parte del estado. Esto reduce el acoplamiento y facilita modificar o sustituir componentes sin afectar al resto del sistema. Por ello, se recomienda utilizar herencia únicamente cuando exista una relación clara de especialización, y preferir la composición cuando el objetivo sea simplemente reutilizar comportamiento.

## 11. Herencia vs. Composición. Se dice que se debe *"favorecer la composición frente a la herencia"*, ¿por qué?

Se recomienda favorecer la composición frente a la herencia porque la composición proporciona un diseño más flexible y menos acoplado. Mientras que la herencia establece una relación rígida entre clases, la composición permite construir objetos a partir de otros sin imponer una jerarquía estricta. Esto facilita cambiar partes del sistema sin afectar a toda la estructura, ya que los componentes pueden sustituirse o modificarse de forma independiente.

La herencia introduce una dependencia fuerte entre la subclase y la superclase. La subclase no solo reutiliza código, sino que también queda ligada al diseño y a la implementación de la clase base. Si esta cambia, puede afectar al comportamiento de todas las clases derivadas. Además, la herencia es estática, ya que la relación se fija en tiempo de compilación y no puede modificarse dinámicamente, lo que limita la capacidad de adaptación del sistema.

La composición, en cambio, permite reutilizar comportamiento delegando en otros objetos. Esto hace que el sistema sea más modular y más fácil de extender. En lugar de heredar funcionalidad, se puede incluir un objeto que la proporcione, lo que permite cambiar esa funcionalidad sin alterar la estructura de clases. Por esta razón, la composición suele ser la opción preferida cuando no existe una relación clara de especialización, ya que permite construir diseños más mantenibles y evolutivos.

## 12. Herencia vs. Composición. Se dice que la *"herencia rompe la encapsulación"*, ¿a qué se refiere esto?

Se dice que la herencia rompe la encapsulación porque permite que las subclases dependan de detalles internos de la superclase más allá de su interfaz pública. Aunque la encapsulación busca ocultar la implementación interna y exponer solo una interfaz controlada, la herencia introduce una relación más estrecha en la que la subclase puede verse afectada por cómo está implementada la clase base. Esto hace que la subclase no dependa solo del “qué hace” la superclase, sino también, en cierta medida, de “cómo lo hace”.

En particular, la subclase puede utilizar miembros protegidos o asumir ciertos comportamientos internos de la superclase. Esto implica que cambios en la implementación de la clase base, incluso sin modificar su interfaz pública, pueden romper el funcionamiento de las subclases. Por tanto, la encapsulación se debilita, ya que la clase base ya no puede cambiar libremente su implementación sin tener en cuenta a las clases que heredan de ella.

Este problema no aparece con la composición, donde una clase utiliza otra únicamente a través de su interfaz pública. En ese caso, los detalles internos permanecen completamente ocultos y pueden modificarse sin afectar al resto del sistema. Por ello, la herencia introduce una dependencia más profunda que puede comprometer la robustez y la evolución del diseño si no se utiliza con cuidado.

## 13. Pongamos un ejemplo de dos alternativas para lo mismo. Tenemos un `Estudiante` y un `Trabajador`, ambos tienen datos en común: el DNI y el nombre. Modelemos esto de dos formas: uno por herencia, con una superclase `Persona`, y otro con composición, con una clase `DatosPersonales`. Se debe recibir una instancia de `DatosPersonales` en el constructor de la clase `Estudiante` y `Trabajador`.

Una forma de modelar la situación es mediante herencia, definiendo una clase base Persona que contenga los atributos comunes, como el DNI y el nombre. A partir de ella, se pueden definir las clases Estudiante y Trabajador como subclases que heredan esos datos. Este enfoque refleja una relación de tipo “A es-un B”, es decir, un estudiante es una persona y un trabajador también lo es. Permite reutilizar directamente el estado y comportamiento común sin duplicar código.

Sin embargo, esta solución introduce una relación jerárquica fuerte entre las clases. Estudiante y Trabajador quedan acoplados a la implementación de Persona, lo que puede dificultar cambios futuros. Además, se asume que ambos tipos son especializaciones de una misma categoría, lo cual puede no ser siempre adecuado dependiendo del contexto del problema. A continuación se muestra una posible implementación con herencia:

class Persona {
    private String dni;
    private String nombre;

    public Persona(String dni, String nombre) {
        this.dni = dni;
        this.nombre = nombre;
    }

    public String getDni() {
        return dni;
    }

    public String getNombre() {
        return nombre;
    }
}

class Estudiante extends Persona {
    public Estudiante(String dni, String nombre) {
        super(dni, nombre);
    }
}

class Trabajador extends Persona {
    public Trabajador(String dni, String nombre) {
        super(dni, nombre);
    }
}