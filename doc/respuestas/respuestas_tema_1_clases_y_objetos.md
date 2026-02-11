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

Las cuatro características básicas de la programación orientada a objetos son encapsulación, abstracción, herencia y polimorfismo. Estas propiedades permiten organizar el código de manera más estructurada que en la programación estructurada tradicional, donde solo se dispone de funciones y datos separados.

La encapsulación consiste en agrupar datos y funciones dentro de una misma unidad llamada clase. En lugar de tener variables globales y funciones independientes como en C, se define una clase que contiene atributos (datos) y métodos (funciones). Además, se puede controlar el acceso a esos datos mediante modificadores como private o public, evitando accesos indebidos y mejorando la seguridad del programa.

La abstracción permite representar únicamente los aspectos esenciales de un objeto, ocultando los detalles internos de implementación. Se trabaja con una idea general del objeto (por ejemplo, “CuentaBancaria”) sin necesidad de conocer cómo están implementadas internamente sus operaciones. Esto facilita el uso del código sin necesidad de entender toda su complejidad interna.

La herencia permite crear una clase nueva a partir de otra existente, reutilizando sus características. Por ejemplo, se puede definir una clase general Vehiculo y luego crear clases más específicas como Coche o Moto que hereden sus atributos y métodos. Por último, el polimorfismo permite que diferentes clases respondan de forma distinta a un mismo método. Es decir, se puede utilizar una misma interfaz o método general y que cada clase lo implemente según su comportamiento específico.

## 2. Cita cuatro lenguajes populares que permitan la programación orientada a objetos

Existen numerosos lenguajes de programación que permiten aplicar el paradigma orientado a objetos. Entre los más populares se encuentran Java, C++, Python y C#, todos ellos ampliamente utilizados en el desarrollo de software profesional y académico.

Java fue diseñado desde sus inicios con un enfoque claramente orientado a objetos, por lo que prácticamente todo se estructura en clases y objetos. C++, por su parte, es una evolución del lenguaje C que incorpora características de orientación a objetos como clases, herencia y polimorfismo, permitiendo combinar programación estructurada y orientada a objetos en un mismo proyecto.

Python también soporta programación orientada a objetos y se caracteriza por su sintaxis sencilla y legible, lo que facilita el aprendizaje del paradigma. Finalmente, C# es un lenguaje desarrollado por Microsoft que, al igual que Java, está fuertemente basado en clases y se utiliza ampliamente en aplicaciones empresariales, desarrollo de escritorio y videojuegos.

## 3. Los paradigmas anteriores a la POO, ¿Qué es la **programación estructurada**? y, todavía mejor, ¿Qué es la **programación modular**?

La programación estructurada es un paradigma anterior a la orientación a objetos que organiza el programa como un conjunto de instrucciones agrupadas en funciones o procedimientos. Se basa en el uso de estructuras de control bien definidas, como secuencias, condicionales (`if`, `switch`) y bucles (`for`, `while`), evitando el uso indiscriminado de saltos como `goto`. En este modelo, los datos y las funciones están separados, y el programa se entiende como una secuencia de pasos que transforman datos a lo largo del tiempo.

En este enfoque, se trabaja principalmente con variables, estructuras (`struct` en C) y funciones independientes. No existe el concepto de clase ni de objeto, por lo que los datos suelen pasarse como parámetros a las funciones o declararse como variables globales. Esto puede provocar que el control sobre el acceso a los datos sea más difícil de mantener en programas grandes.

La programación modular es una evolución dentro del paradigma estructurado que busca dividir el programa en partes más pequeñas y organizadas llamadas módulos. Cada módulo agrupa funciones relacionadas con una tarea concreta, favoreciendo la organización, reutilización y mantenimiento del código. En C, por ejemplo, se implementa mediante la separación en distintos archivos `.c` y `.h`, donde cada uno contiene funciones y definiciones específicas.

Mientras que la programación estructurada se centra en cómo se organizan las instrucciones, la programación modular pone el énfasis en cómo se divide el programa en bloques funcionales independientes. Sin embargo, en ambos casos los datos y las funciones siguen estando conceptualmente separados, a diferencia de la programación orientada a objetos, donde ambos se integran dentro de las clases.

## 4. ¿Qué tres elementos definen a un objeto en programación orientada a objetos?

En programación orientada a objetos, un objeto queda definido por tres elementos fundamentales: estado, comportamiento e identidad. Estos tres aspectos permiten diferenciarlo de una simple variable o estructura de datos, como las utilizadas en programación estructurada.

El estado está formado por los datos que describen al objeto en un momento determinado. En Java, estos datos se almacenan en los atributos o variables de instancia de la clase. Por ejemplo, si se define un objeto `Coche`, su estado podría estar compuesto por atributos como color, velocidad o matrícula. En comparación con C, sería similar a una `struct`, pero con la diferencia de que en orientación a objetos estos datos están ligados a funciones propias del objeto.

El comportamiento está determinado por los métodos que puede ejecutar el objeto. Los métodos son funciones asociadas a la clase y permiten modificar o consultar el estado del objeto. Siguiendo el ejemplo anterior, un coche podría tener métodos como `acelerar()` o `frenar()`. A diferencia de la programación estructurada, donde las funciones son independientes, aquí el comportamiento está directamente vinculado al objeto.

Por último, la identidad permite distinguir un objeto de otro, incluso si ambos tienen el mismo estado. Es decir, dos objetos pueden tener exactamente los mismos valores en sus atributos, pero siguen siendo instancias distintas en memoria. Esta identidad se gestiona internamente por el sistema y es lo que permite trabajar con múltiples objetos del mismo tipo sin que se confundan entre sí.

## 5. ¿Qué es una clase? ¿Es lo mismo que un objeto? ¿Qué es una instancia? ¿Todos los lenguajes orientados a objetos manejan el concepto de clase?

Una clase es una plantilla o modelo que define cómo serán los objetos de un determinado tipo. En ella se especifican los atributos (datos) y los métodos (funciones) que tendrán los objetos creados a partir de esa clase. En comparación con C, podría entenderse como una evolución de una struct, pero ampliada, ya que no solo agrupa datos, sino también el comportamiento asociado a esos datos.

Una clase no es lo mismo que un objeto. La clase es la definición general, mientras que el objeto es una entidad concreta creada a partir de esa definición. Por ejemplo, si se define la clase Coche, esta describe qué propiedades y acciones tendrá cualquier coche; sin embargo, un coche específico con matrícula concreta y color determinado sería un objeto. En este contexto, una instancia es simplemente un objeto creado a partir de una clase. Por tanto, “objeto” e “instancia” suelen utilizarse como términos equivalentes.

No todos los lenguajes orientados a objetos manejan obligatoriamente el concepto de clase. Lenguajes como Java, C++ o C# están basados en clases, por lo que toda la estructura del programa gira en torno a ellas. Sin embargo, existen lenguajes orientados a objetos basados en prototipos, como JavaScript, donde no se utilizan clases en el sentido tradicional, sino objetos que pueden servir como modelo para otros objetos. Esto demuestra que la orientación a objetos no depende estrictamente del concepto de clase, aunque en muchos lenguajes sea el mecanismo principal para implementarla.

## 6. ¿Dónde se almacenan en memoria los objetos? ¿Es igual en todos los lenguajes? ¿Qué es la **recolección de basura**? 

En la mayoría de los lenguajes orientados a objetos, los objetos se almacenan en una zona de memoria llamada heap (montículo). Cuando se crea un objeto en Java utilizando `new`, se reserva espacio en el heap para guardar sus atributos. La variable que se declara en el código no contiene el objeto en sí, sino una referencia (similar a un puntero en C) que apunta a la posición donde el objeto está almacenado en memoria. Esta diferencia es importante para entender cómo se comparten y modifican los objetos.

No es exactamente igual en todos los lenguajes. En C++, por ejemplo, un objeto puede almacenarse en la pila (stack) si se declara de forma directa, o en el heap si se reserva dinámicamente con `new`. En cambio, en Java todos los objetos se almacenan en el heap y solo las variables primitivas y las referencias pueden estar en la pila. Por tanto, la gestión de memoria depende del lenguaje y de su diseño interno.

La recolección de basura (garbage collection) es un mecanismo automático de gestión de memoria. Consiste en liberar automáticamente el espacio ocupado por objetos que ya no están siendo utilizados por el programa. En lenguajes como Java, no es necesario liberar la memoria manualmente, ya que el sistema detecta qué objetos ya no tienen referencias activas y recupera su espacio de forma automática. Esto reduce errores comunes en C/C++, como fugas de memoria o liberaciones incorrectas.

## 7. ¿Qué es un método? ¿Qué es la **sobrecarga de métodos**? 

Un método es una función definida dentro de una clase y asociada a los objetos creados a partir de ella. Representa el comportamiento que puede ejecutar un objeto y, normalmente, actúa sobre los atributos de dicho objeto. En comparación con C, puede entenderse como una función, pero con la diferencia de que está ligada a una clase y puede acceder directamente a sus datos internos. En Java, los métodos se definen dentro de la clase y pueden recibir parámetros y devolver un valor, igual que las funciones tradicionales.

La sobrecarga de métodos consiste en definir varios métodos dentro de la misma clase que tienen el mismo nombre, pero se diferencian en el número o tipo de parámetros que reciben. El compilador distingue cuál debe ejecutarse en función de los argumentos utilizados en la llamada. Esto permite utilizar un mismo nombre lógico para realizar operaciones similares, manteniendo el código más claro y organizado.

Por ejemplo, se puede definir un método `sumar(int a, int b)` y otro `sumar(double a, double b)`. Aunque ambos se llaman igual, el sistema decide cuál usar dependiendo del tipo de datos proporcionados. Es importante señalar que la sobrecarga no depende del tipo de dato que se devuelve, sino únicamente de los parámetros. De esta forma, se amplía la flexibilidad del código sin necesidad de cambiar los nombres de los métodos.

## 8. Ejemplo mínimo de clase en Java, que se llame Punto, con dos atributos, x e y, con un método que se llame `calculaDistanciaAOrigen`, que calcule la distancia a la posición 0,0. Por sencillez, los atributos deben tener visibilidad por defecto. Crea además un ejemplo de uso con una instancia y uso del método

class Punto {
    double x;  // visibilidad por defecto
    double y;  // visibilidad por defecto

    double calculaDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }
}

public class PruebaPunto {
    public static void main(String[] args) {
        Punto p = new Punto();
        p.x = 3;
        p.y = 4;

        double distancia = p.calculaDistanciaAOrigen();
        System.out.println("Distancia al origen: " + distancia);
    }
}

## 9. ¿Cuál es el punto de entrada en un programa en Java? ¿Qué es `static` y para qué vale? ¿Sólo se emplea para ese método `main`? ¿Para qué se combina con `final`?

El punto de entrada de un programa en Java es el método main. Se trata de un método especial cuya firma habitual es public static void main(String[] args). Cuando se ejecuta el programa, la máquina virtual de Java (JVM) busca automáticamente ese método para comenzar la ejecución. A diferencia de C, donde también existe una función main, en Java este método debe estar dentro de una clase y cumplir exactamente esa estructura para que el programa pueda iniciarse correctamente.

La palabra clave static indica que un atributo o método pertenece a la clase y no a una instancia concreta. Es decir, puede utilizarse sin necesidad de crear un objeto previamente. Por ejemplo, el método main es static porque se necesita ejecutar antes de que exista ningún objeto. Del mismo modo, métodos como Math.sqrt() son estáticos, ya que se llaman directamente sobre la clase Math sin crear una instancia.

El modificador static no se emplea únicamente en el método main. Puede utilizarse en atributos (variables estáticas compartidas por todas las instancias), en métodos (que no dependen del estado de un objeto) e incluso en bloques de inicialización. Su uso es frecuente cuando se necesita un comportamiento común a todos los objetos de una clase o cuando no tiene sentido asociarlo a una instancia concreta.

Cuando static se combina con final, se indica que ese miembro pertenece a la clase y, además, no puede modificarse una vez inicializado. Esto se utiliza comúnmente para definir constantes compartidas, como por ejemplo public static final double PI = 3.1416;. En este caso, el valor es único para toda la clase y no puede cambiar durante la ejecución del programa.

## 10. Intenta ejecutar un poco de Java de forma básica, con los comandos `javac` y `java`. ¿Cómo podemos compilar el programa y ejecutarlo desde linea de comandos? ¿Java es compilado? ¿Qué es la **máquina virtual**? ¿Qué es el *byte-code* y los ficheros `.class`?

Para compilar y ejecutar un programa en Java desde línea de comandos se utilizan dos herramientas principales: `javac` y `java`. Primero, se escribe el código en un fichero con extensión `.java`, por ejemplo `Prueba.java`. Para compilarlo, se ejecuta el comando `javac Prueba.java`. Si no hay errores, se generará un nuevo fichero llamado `Prueba.class`. Posteriormente, para ejecutar el programa se utiliza el comando `java Prueba` (sin la extensión `.class`). Es importante que el nombre del archivo coincida con el de la clase pública definida en el código.

Java es un lenguaje compilado, pero no en el mismo sentido tradicional que C o C++. Cuando se usa `javac`, el código fuente no se transforma directamente en código máquina específico del sistema operativo, sino en un formato intermedio llamado byte-code. Este byte-code es independiente de la plataforma, lo que permite que el mismo programa pueda ejecutarse en distintos sistemas sin necesidad de recompilarlo.

La máquina virtual de Java (JVM) es el componente encargado de ejecutar ese byte-code. Actúa como una capa intermedia entre el programa y el sistema operativo. La JVM interpreta o compila dinámicamente el byte-code a código máquina específico del equipo donde se ejecuta. Gracias a este mecanismo, se cumple el principio “write once, run anywhere”, es decir, escribir una vez y ejecutar en cualquier plataforma que disponga de una JVM compatible.

El byte-code es, por tanto, un código intermedio generado por el compilador `javac`. Se almacena en archivos con extensión `.class`, que contienen instrucciones en un formato que la JVM puede entender. Estos ficheros no son ejecutables directos del sistema operativo, sino que requieren la máquina virtual para su ejecución. De este modo, Java combina características de lenguajes compilados y lenguajes interpretados.

## 11. En el código anterior de la clase `Punto` ¿Qué es `new`? ¿Qué es un **constructor**? Pon un ejemplo de constructor en una clase `Empleado` que tenga DNI, nombre y apellidos

En el código de la clase Punto, la palabra clave new se utiliza para crear un objeto en memoria. Al ejecutarse new Punto(), se reserva espacio en el heap para almacenar los atributos del objeto y se devuelve una referencia a esa posición de memoria. En comparación con C, podría entenderse como una reserva dinámica de memoria similar a malloc, pero en este caso ya asociada al tipo definido por la clase y gestionada automáticamente por el sistema.

Un constructor es un método especial que se ejecuta automáticamente cuando se crea un objeto con new. Su función principal es inicializar los atributos del objeto. El constructor tiene el mismo nombre que la clase y no tiene tipo de retorno (ni siquiera void). Permite asegurar que el objeto se cree en un estado válido desde el primer momento, evitando que sus atributos queden sin inicializar o con valores incoherentes.

En este ejemplo, el uso de this permite distinguir los atributos de la clase de los parámetros del constructor cuando tienen el mismo nombre. Al crear un objeto mediante new Empleado("12345678A", "Ana", "García López"), se ejecutará automáticamente el constructor, inicializando los datos del empleado en el momento de su creación. A continuación se muestra un ejemplo de clase Empleado con un constructor que recibe como parámetros el DNI, el nombre y los apellidos, y los asigna a los atributos correspondientes:

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

## 12. ¿Qué es la referencia `this`? ¿Se llama igual en todos los lenguajes? Pon un ejemplo del uso de `this` en la clase `Punto`

La referencia this es una variable implícita que hace referencia al objeto actual, es decir, a la instancia concreta sobre la que se está ejecutando un método o constructor. Permite acceder a los atributos y métodos del propio objeto cuando existe ambigüedad con variables locales o parámetros. En términos conceptuales, podría compararse con un puntero implícito al objeto actual, similar al puntero this que existe en C++ dentro de las clases.

No todos los lenguajes utilizan exactamente el mismo nombre, aunque el concepto es común en la mayoría de lenguajes orientados a objetos basados en clases. En Java y C++ se utiliza this, mientras que en otros lenguajes pueden existir variaciones o contextos donde no es necesario escribirlo explícitamente. En cualquier caso, la idea subyacente es siempre la misma: referirse al objeto actual que está ejecutando el código.

En el constructor, this.x y this.y permiten diferenciar los atributos de la clase de los parámetros recibidos. Sin this, el nombre x haría referencia únicamente al parámetro local. De esta forma, se garantiza que los valores recibidos se asignen correctamente al estado del objeto que se está creando. A continuación se muestra un ejemplo de uso de this en la clase Punto, especialmente útil cuando el nombre de los parámetros coincide con el de los atributos:

class Punto {
    double x;
    double y;

    Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    double calculaDistanciaAOrigen() {
        return Math.sqrt(this.x * this.x + this.y * this.y);
    }
}

## 13. Añade ahora otro nuevo método que se llame `distanciaA`, que reciba un `Punto` como parámetro y calcule la distancia entre `this` y el punto proporcionado

Se puede añadir a la clase Punto un nuevo método llamado distanciaA que reciba como parámetro otro objeto de tipo Punto. Este método calculará la distancia entre el punto actual (referenciado mediante this) y el punto pasado como argumento. Para ello, se aplica la fórmula de la distancia entre dos puntos en el plano: √((x₂ − x₁)² + (y₂ − y₁)²).

Desde el punto de vista conceptual, el método utiliza los atributos del objeto actual (this.x, this.y) y los compara con los atributos del objeto recibido como parámetro. Se observa que, en orientación a objetos, resulta natural que el propio objeto contenga el comportamiento necesario para interactuar con otros objetos del mismo tipo. Esto evita tener funciones externas que operen sobre estructuras, como ocurriría en programación estructurada.

A continuación se muestra la clase Punto con el nuevo método incorporado:

class Punto {
    double x;
    double y;

    Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    double calculaDistanciaAOrigen() {
        return Math.sqrt(this.x * this.x + this.y * this.y);
    }

    double distanciaA(Punto otro) {
        double dx = this.x - otro.x;
        double dy = this.y - otro.y;
        return Math.sqrt(dx * dx + dy * dy);
    }
}

## 14. El paso del `Punto` como parámetro a un método, es **por copia** o **por referencia**, es decir, si se cambia el valor de algún atributo del punto pasado como parámetro, dichos cambios afectan al objeto fuera del método? ¿Qué ocurre si en vez de un `Punto`, se recibiese un entero (`int`) y dicho entero se modificase dentro de la función? 

En Java, el paso de parámetros siempre se realiza por valor, pero es importante distinguir qué significa eso cuando se trabaja con objetos. Cuando se pasa un objeto como parámetro (por ejemplo, un Punto), lo que se copia no es el objeto completo, sino la referencia al objeto. Es decir, el método recibe una copia de la referencia que apunta al mismo objeto en memoria. Por tanto, si dentro del método se modifican los atributos del objeto recibido, esos cambios sí afectan al objeto original, ya que ambos apuntan al mismo contenido en el heap.

Sin embargo, si dentro del método se reasigna la referencia (por ejemplo, asignándola a un nuevo objeto), ese cambio no afecta a la referencia original fuera del método. Esto se debe a que la variable parámetro es solo una copia de la referencia original, y modificar esa copia no altera la variable que estaba en el ámbito exterior.

En cambio, si en lugar de un objeto se recibe un tipo primitivo como int, el valor que se copia es el propio dato. En este caso, cualquier modificación realizada dentro del método afecta únicamente a la copia local del parámetro y no tiene ningún efecto fuera del método. Por tanto, al finalizar la ejecución, el valor original permanece inalterado. Esto marca una diferencia clara entre el comportamiento de los tipos primitivos y el de los objetos en Java.

## 15. ¿Qué es el método `toString()` en Java? ¿Existe en otros lenguajes? Pon un ejemplo de `toString()` en la clase `Punto` en Java

El método toString() en Java es un método definido en la clase base Object, de la cual heredan todas las clases. Su finalidad es proporcionar una representación en forma de texto del objeto. Cuando se intenta imprimir un objeto directamente (por ejemplo, mediante System.out.println(objeto)), Java invoca automáticamente su método toString(). Si no se redefine, la implementación por defecto devuelve una cadena que incluye el nombre de la clase y un identificador interno, lo cual suele no ser informativo para el usuario.

Este concepto existe en muchos otros lenguajes orientados a objetos, aunque puede recibir nombres distintos. En C++, por ejemplo, se puede sobrecargar el operador de inserción (<<) para definir cómo se muestra un objeto en un flujo de salida. En Python, existen métodos especiales como __str__() y __repr__() con una finalidad similar. En todos los casos, la idea es permitir que cada clase defina su propia representación textual.

A continuación se muestra un ejemplo de cómo se podría redefinir toString() en la clase Punto para que muestre sus coordenadas de forma clara:
class Punto {
    double x;
    double y;

    @Override
    public String toString() {
        return "Punto(" + x + ", " + y + ")";
    }
}

## 16. Reflexiona: ¿una clase es como un `struct` en C? ¿Qué le falta al `struct` para ser como una clase y las variables de ese tipo ser instancias?

Una clase puede compararse parcialmente con un struct en C, ya que ambos permiten agrupar varios datos bajo un mismo tipo. En ese sentido, una clase también define una estructura de datos compuesta por distintos atributos. Sin embargo, la comparación es incompleta, porque una clase no solo agrupa datos, sino también comportamiento asociado a esos datos.

Lo que le falta a un struct tradicional en C para ser equivalente a una clase es la integración directa de funciones dentro de su definición. En C, las funciones se declaran de forma independiente y reciben como parámetro un puntero a la estructura si se desea operar sobre ella. En cambio, en una clase, los métodos forman parte del propio tipo y tienen acceso directo a sus atributos, lo que establece una relación más fuerte entre datos y comportamiento.

Además, un struct no incorpora de forma nativa mecanismos como encapsulación (control de acceso mediante public, private, etc.), constructores automáticos, herencia o polimorfismo. Para que un struct fuese equivalente a una clase en sentido completo, debería permitir definir métodos internos, controlar el acceso a sus miembros y soportar mecanismos de reutilización y especialización. Por ello, aunque ambos conceptos comparten la idea de agrupar datos, la clase representa una abstracción más completa y potente dentro de la programación orientada a objetos.

## 17. Quitemos un poco de magia a todo esto: ¿Como se podría “emular”, con `struct` en C, la clase `Punto`, con su función para calcular la distancia al origen? ¿Qué ha pasado con `this`?

En C se puede “emular” una clase separando explícitamente datos y funciones: el `struct` contendría únicamente los campos (por ejemplo `x` e `y`) y, aparte, se definiría una función que opere sobre ese `struct`. En la práctica, se obtiene un estilo “orientado a objetos manual”, donde cada operación que en Java sería un método se convierte en una función que recibe el “objeto” como argumento.

Al no existir métodos asociados al tipo, la función debe recibir un parámetro que indique sobre qué punto se calcula la distancia. Normalmente se pasa un puntero o una copia del `struct` (según se quiera modificar o no), y se accede a sus campos desde la función. Así se reproduce el comportamiento de `calculaDistanciaAOrigen`, pero sin que el propio tipo contenga la lógica.

Lo que en Java aparece como `this` no es más que la referencia implícita al objeto actual. En C esa “magia” desaparece: `this` se convierte en el primer parámetro explícito de la función (por ejemplo, `Punto* p` o `struct Punto* p`). Es decir, el programador debe pasar manualmente el “objeto actual” cada vez que se llama a la función, porque C no añade automáticamente ninguna referencia al `struct` que está “ejecutando” la operación.
