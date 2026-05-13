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

Un puntero a una función es una variable que almacena la dirección de memoria de una función. Gracias a ello, la función puede tratarse como un dato más, permitiendo almacenarla en variables, pasarla como parámetro o devolverla desde otras funciones. Esto aporta flexibilidad, ya que el comportamiento que se ejecutará puede decidirse dinámicamente durante la ejecución del programa.
En C, un puntero a función debe declarar explícitamente el tipo de parámetros y el tipo de retorno de la función a la que apunta. Una vez inicializado con la dirección de una función compatible, puede utilizarse para invocar dicha función igual que si se llamase directamente. Este mecanismo es muy utilizado para implementar callbacks, tablas de funciones o comportamientos configurables.
A continuación se muestra un ejemplo donde se define una función que convierte una cadena a mayúsculas. Después se crea un puntero a función llamado aMayusculas y se utiliza para invocar la función a través del puntero:

#include <stdio.h>
#include <ctype.h>

void convertirAMayusculas(char* texto) {
    int i = 0;

    while (texto[i] != '\0') {
        texto[i] = toupper(texto[i]);
        i++;
    }
}

int main() {
    char mensaje[] = "hola mundo";

    void (*aMayusculas)(char*) = convertirAMayusculas;

    aMayusculas(mensaje);

    printf("%s\n", mensaje);

    return 0;
}

## 2. ¿Qué es una **función lambda** en un lenguaje de programación? Pon un ejemplo similar al anterior en Javascript y otro en Java con funciones lambda. Usa una variable local `aMayusculas` para apuntar a la función lambda. Por simplicidad, en Java, emplea `Function<String, String>` para el tipo de la referencia a la función lambda.

Una función lambda es una función anónima que puede definirse directamente como un valor y almacenarse en variables, pasarse como parámetro o devolverse desde otras funciones. Su principal objetivo es permitir definir comportamientos de forma más breve y flexible, especialmente cuando se necesita una función sencilla y puntual. Conceptualmente, cumplen un papel similar al de los punteros a función en C, pero integrados dentro del propio sistema de objetos y tipos del lenguaje.

En lenguajes modernos, las funciones lambda permiten tratar funciones como datos. Esto facilita estilos de programación más flexibles y expresivos, especialmente en operaciones sobre colecciones, callbacks o programación funcional. La sintaxis concreta depende del lenguaje, pero la idea común es definir una función sin necesidad de darle un nombre explícito.

A continuación se muestra un ejemplo en Javascript donde se define una función lambda que transforma una cadena a mayúsculas y se almacena en una variable local llamada aMayusculas:

const aMayusculas = (texto) => {
    return texto.toUpperCase();
};

console.log(aMayusculas("hola mundo"));

En Java también existen funciones lambda, aunque deben asociarse a interfaces funcionales. En este caso se utiliza Function<String, String>, que representa una función que recibe un String y devuelve otro String:

import java.util.function.Function;

public class Main {
    public static void main(String[] args) {

        Function<String, String> aMayusculas =
                texto -> texto.toUpperCase();

        System.out.println(aMayusculas.apply("hola mundo"));
    }
}

## 3. ¿Qué es el **paradigma funcional**? ¿Por qué a algunos lenguajes orientados a objetos como Java 8, se les llama multi-paradigma? ¿Qué quiere decir que las funciones son "ciudadanos de primera clase"?

El paradigma funcional es un estilo de programación basado en el uso de funciones como elemento principal para construir programas. En lugar de centrarse en objetos y cambios de estado, se basa en aplicar funciones sobre datos y producir nuevos resultados sin modificar los valores originales. En este paradigma se intenta reducir los efectos secundarios y trabajar con funciones puras, es decir, funciones que para los mismos parámetros siempre producen el mismo resultado y no alteran el estado externo del programa.

Tradicionalmente, lenguajes como Java estaban enfocados principalmente al paradigma orientado a objetos. Sin embargo, desde Java 8 se incorporaron características propias del paradigma funcional, como las funciones lambda, referencias a métodos y operaciones funcionales sobre colecciones. Por esta razón se considera un lenguaje multi-paradigma, ya que permite combinar distintas formas de programación dentro del mismo lenguaje, utilizando tanto objetos y herencia como técnicas funcionales.

Decir que las funciones son “ciudadanos de primera clase” significa que las funciones pueden tratarse igual que otros datos del lenguaje. Es decir, pueden almacenarse en variables, pasarse como parámetro, devolverse desde otras funciones y formar parte de estructuras de datos. Esta capacidad aporta mucha flexibilidad y es una de las características fundamentales de los lenguajes funcionales y de las extensiones funcionales añadidas a lenguajes como Java o Javascript.

## 4. Explica la sintaxis básica de una función lambda en Java.

La sintaxis básica de una función lambda en Java permite definir una función de forma compacta y anónima, sin necesidad de crear explícitamente una clase con un método. Una expresión lambda se compone principalmente de una lista de parámetros, el operador -> y el cuerpo de la función. La parte situada antes de la flecha representa los parámetros de entrada y la parte posterior representa la operación que se ejecutará.

La forma más sencilla tiene la estructura:

(parametros) -> expresion

Si el cuerpo contiene una única expresión, el valor se devuelve automáticamente. En cambio, si el cuerpo contiene varias instrucciones, debe escribirse entre llaves y utilizar return cuando sea necesario. Además, cuando existe un único parámetro, pueden omitirse los paréntesis, y cuando el compilador puede inferir los tipos, tampoco es necesario especificarlos explícitamente.

Las funciones lambda en Java no existen de manera independiente, sino que siempre se asocian a una interfaz funcional, es decir, una interfaz que contiene un único método abstracto. La lambda proporciona la implementación de ese método. Por ejemplo:

Function<String, String> aMayusculas =
texto -> texto.toUpperCase();

En este caso, la lambda recibe un String llamado texto y devuelve el resultado de convertirlo a mayúsculas. El compilador entiende automáticamente que esta lambda implementa el método apply de la interfaz Function<String, String>.

## 5. Ahora recibamos una función como parámetro a un método y la llamaremos desde dentro. Amplia los ejemplos anteriores de Java y JavaScript con un método llamado `transformar`, que reciba un `String` como parámetro y luego una función transformadora como lo es `aMayúsculas` y la invoque desde dentro.

Una de las ventajas principales de las funciones lambda es que permiten pasar comportamiento como parámetro a otros métodos. Esto significa que una función puede recibirse igual que cualquier otro dato y ejecutarse desde dentro del método receptor. Este mecanismo es muy utilizado en programación funcional y permite construir código más flexible y reutilizable, ya que el comportamiento concreto puede decidirse externamente.

En JavaScript esto resulta especialmente natural, porque las funciones son ciudadanos de primera clase. Un método puede recibir otra función como argumento y ejecutarla directamente. En el siguiente ejemplo, el método transformar recibe un texto y una función transformadora, que posteriormente aplica al contenido recibido:

const aMayusculas = (texto) => {
    return texto.toUpperCase();
};

function transformar(texto, transformadora) {
    return transformadora(texto);
}

console.log(transformar("hola mundo", aMayusculas));

En Java ocurre algo similar, aunque las funciones lambda deben asociarse a interfaces funcionales. En este caso se utiliza Function<String, String>, que representa una función que recibe un String y devuelve otro String. El método transformar recibe tanto el texto como la función transformadora y ejecuta dicha función desde dentro:

import java.util.function.Function;

public class Main {

    public static String transformar(
            String texto,
            Function<String, String> transformadora) {

        return transformadora.apply(texto);
    }

    public static void main(String[] args) {

        Function<String, String> aMayusculas =
                t -> t.toUpperCase();

        String resultado =
                transformar("hola mundo", aMayusculas);

        System.out.println(resultado);
    }
}

## 6. Ahora, invoca `transformar`, con una nueva función lambda directamente en la llamada a `transformar`, por ejemplo, una función lambda que invierta la cadena. Define la función de inversión justo cuando la estás pasando como parámetro.

Las funciones lambda también pueden definirse directamente en el momento de la llamada a un método, sin necesidad de almacenarlas previamente en una variable. Esto resulta útil cuando la función solo va a utilizarse una vez, ya que permite escribir el comportamiento justo donde se necesita. Este estilo es muy habitual en programación funcional y hace el código más compacto y expresivo.

En este caso, el método transformar sigue recibiendo un String y una función transformadora, pero ahora la función lambda se define directamente en la llamada. La lambda invierte la cadena recibida y devuelve el resultado transformado.

Ejemplo en JavaScript:

function transformar(texto, transformadora) {
    return transformadora(texto);
}

const resultado = transformar(
    "hola mundo",
    texto => texto.split("").reverse().join("")
);

console.log(resultado);

Ejemplo equivalente en Java:

import java.util.function.Function;

public class Main {

    public static String transformar(
            String texto,
            Function<String, String> transformadora) {

        return transformadora.apply(texto);
    }

    public static void main(String[] args) {

        String resultado = transformar(
            "hola mundo",
            texto -> new StringBuilder(texto).reverse().toString()
        );

        System.out.println(resultado);
    }
}

## 7. ¿Qué se entiende por cierre o "closure" en el contexto de las funciones lambda? Pon un ejemplo en Java de cómo una función lambda es capaz de acceder a una variable local en el contexto donde fue definida. Modifica el ejemplo anterior, creando otra función lambda para transformar una cadena, pero que lo que haga es concatenar a la cadena de entrada otra cadena que está en una variable local definida fuera de la función lambda.

Un cierre o closure es una función que puede acceder a variables definidas en el contexto donde fue creada, incluso aunque esas variables no pertenezcan directamente a la función. Esto significa que la función lambda “captura” parte del entorno exterior y puede utilizar esos valores posteriormente. Gracias a ello, las funciones pueden personalizar su comportamiento utilizando datos externos sin necesidad de recibirlos explícitamente como parámetros.

En Java, las funciones lambda pueden acceder a variables locales del contexto donde se definen, siempre que dichas variables sean finales o efectivamente finales, es decir, que no cambien después de su inicialización. El compilador captura el valor de esas variables y lo hace accesible desde el interior de la lambda. Este mecanismo permite crear funciones más flexibles y expresivas manteniendo la seguridad del lenguaje.

A continuación se modifica el ejemplo anterior para que la función lambda concatene una cadena adicional almacenada en una variable local externa:

import java.util.function.Function;

public class Main {

    public static String transformar(
            String texto,
            Function<String, String> transformadora) {

        return transformadora.apply(texto);
    }

    public static void main(String[] args) {

        String sufijo = " !!!";

        Function<String, String> concatenar =
                texto -> texto + sufijo;

        String resultado =
                transformar("Hola mundo", concatenar);

        System.out.println(resultado);
    }
}

## 8. Reflexiona: ¿en qué se diferencia entonces una función lambda de los punteros a funciones que hay en C?

Aunque ambos mecanismos permiten tratar funciones como valores y pasarlas a otras partes del programa, las funciones lambda son conceptualmente más potentes y flexibles que los punteros a función de C. Un puntero a función en C simplemente almacena la dirección de memoria de una función ya existente. No contiene estado adicional ni información sobre el contexto donde fue creado. Por tanto, únicamente permite invocar una función concreta a través de una referencia indirecta.

Las funciones lambda, en cambio, pueden comportarse como cierres o closures, capturando variables del entorno donde fueron definidas. Esto significa que una lambda no solo representa código ejecutable, sino también parte del contexto asociado a dicho código. Gracias a ello, una misma estructura de función puede producir comportamientos distintos dependiendo de las variables capturadas, algo que no ocurre con los punteros a función tradicionales de C.

Además, en lenguajes como Java o Javascript, las funciones lambda están integradas en el sistema de tipos y en el paradigma orientado a objetos o funcional del lenguaje. Pueden combinarse con interfaces funcionales, colecciones y mecanismos de programación funcional. Los punteros a función en C son mucho más simples y cercanos al nivel de memoria y direcciones, mientras que las lambdas forman parte de un modelo de abstracción más avanzado y expresivo.

## 9. Devolvamos ahora funciones. Creemos ahora una función que sea capaz de crear funciones "descuento". Una función "descuento", decrementa un porcentaje pasado como parámetro. Por simplicidad, usa `Function<Double, Double>` para su tipo. La función `crearDescuento(porcentaje)`, recibe solo el porcentaje de descuento a aplicar y devuelve la función de descuento. Prueba a crear dos descuentos distintos y aplicarlos a una cantidad. Explica la closure en la función descuento.

Las funciones lambda no solo pueden recibirse como parámetros, sino también devolverse como resultado de otras funciones. Esto permite crear funciones dinámicamente a partir de ciertos parámetros de configuración. En este caso, la función crearDescuento recibe un porcentaje y devuelve otra función capaz de aplicar dicho descuento a una cantidad concreta. Así, se pueden generar distintas funciones de descuento reutilizando la misma lógica general.

La clave de este ejemplo está en el concepto de closure. La función lambda devuelta utiliza la variable porcentaje definida en el contexto de crearDescuento, incluso después de que dicha función haya terminado su ejecución. La lambda captura ese valor y lo conserva asociado a sí misma. Gracias a ello, cada función descuento creada mantiene internamente su propio porcentaje de descuento, produciendo comportamientos distintos aunque todas provengan de la misma función generadora.

import java.util.function.Function;

public class Main {

    public static Function<Double, Double> crearDescuento(double porcentaje) {

        return cantidad -> cantidad - (cantidad * porcentaje / 100.0);
    }

    public static void main(String[] args) {

        Function<Double, Double> descuento10 =
                crearDescuento(10);

        Function<Double, Double> descuento25 =
                crearDescuento(25);

        System.out.println(descuento10.apply(200.0));
        System.out.println(descuento25.apply(200.0));
    }
}

## 10. En Java, que es un lenguaje con comprobación estática de tipos, donde los tipos se declaran, toda función lambda tiene un tipo, que se conoce como **interfaz funcional**. ¿Qué es una **interfaz funcional**? ¿Qué requisitos tiene?

Una interfaz funcional es una interfaz que representa una única operación abstracta y que puede utilizarse como tipo de una función lambda o de una referencia a método. En Java, las funciones lambda no existen como un tipo independiente del lenguaje, sino que siempre se interpretan como una implementación de una interfaz funcional. Por tanto, el tipo real de una lambda es la interfaz funcional a la que se asigna.

El requisito fundamental de una interfaz funcional es tener exactamente un único método abstracto. Esto permite que el compilador pueda asociar sin ambigüedad la función lambda con la implementación de ese método. Aunque solo puede existir un método abstracto, sí pueden existir métodos default, métodos static o métodos heredados de Object, ya que estos no cuentan como métodos abstractos adicionales.

Java proporciona la anotación @FunctionalInterface para indicar explícitamente que una interfaz está diseñada con este propósito. No es obligatoria, pero permite que el compilador verifique que realmente se cumple la restricción de tener un único método abstracto. Las interfaces funcionales son la base del soporte de programación funcional introducido en Java 8 y permiten trabajar con funciones lambda manteniendo la comprobación estática de tipos.

## 11. Creemos una interfaz funcional a mano. Por ejemplo, define la interfaz funcional del ejemplo que transforma la cadena en otra. Llámale `Transformador`, que define una función que convierte una cadena de texto (`String`) en otra (`String`).

Una interfaz funcional personalizada se define igual que cualquier otra interfaz de Java, pero incluyendo un único método abstracto. Ese método representa la operación que posteriormente implementarán las funciones lambda asociadas a dicha interfaz. En este caso, se desea representar una operación que recibe una cadena de texto y devuelve otra cadena transformada.

Al definir la interfaz manualmente, se consigue crear un tipo específico y más expresivo para el problema concreto, en lugar de reutilizar interfaces genéricas ya existentes como Function<String, String>. Además, esto permite adaptar el nombre del método y de la interfaz al dominio del problema, haciendo el código más legible y semántico.

@FunctionalInterface
public interface Transformador {

    String transformar(String texto);
}

## 12. Ahora hagamos la interfaz funcional algo más genérica y empleando generics, para que permita definir un `Transformador` de un tipo en otro. Pon un ejemplo de un transformador que redondea un `Double` en un `Integer`.

Se puede generalizar la interfaz funcional utilizando parámetros de tipo para que no trabaje únicamente con cadenas, sino con cualquier combinación de tipos de entrada y salida. De este modo, la interfaz deja de estar limitada a transformar String en String y pasa a representar una operación genérica que convierte un objeto de un tipo en otro distinto. Esto aumenta considerablemente la reutilización y flexibilidad del diseño.
Para ello se utilizan dos parámetros de tipo, uno para el valor de entrada y otro para el valor de salida. La interfaz ya no depende de tipos concretos, sino que estos se especifican cuando se utiliza. Así, el compilador puede comprobar que las transformaciones son correctas y garantizar la seguridad de tipos sin necesidad de conversiones manuales.

@FunctionalInterface
public interface Transformador<E, S> {

    S transformar(E entrada);
}

public class Main {

    public static void main(String[] args) {

        Transformador<Double, Integer> redondeador =
                valor -> (int) Math.round(valor);

        Integer resultado = redondeador.transformar(3.7);

        System.out.println(resultado);
    }
}

## 13. `Transformador`, en su versión genérica, parece muy útil y reutilizable, hasta el punto de que es igual a una interfaz funcional que ya hay, que es `Function<T, R>`. Muestra las interfaces funcionales predefinidas que hay en Java.

Java incluye una serie de interfaces funcionales predefinidas dentro del paquete java.util.function. Estas interfaces cubren los casos más habituales de programación funcional, evitando que el programador tenga que definir continuamente nuevas interfaces funcionales para operaciones comunes. Todas ellas pueden utilizarse directamente con funciones lambda y referencias a métodos.

La interfaz Function<T, R> representa una función que recibe un valor de tipo T y devuelve otro de tipo R, siendo equivalente a la interfaz Transformador creada anteriormente. Además de esta, existen otras interfaces orientadas a distintos patrones de comportamiento, como consumir datos, producir valores o realizar comprobaciones booleanas. Gracias a estas interfaces estándar, el código resulta más uniforme y compatible con las APIs funcionales de Java.

Estas interfaces son la base de gran parte de la programación funcional en Java, especialmente en streams, colecciones y operaciones sobre datos.

## 14. Vamos a ver ejemplos expresivos de funcional en Java. Estudiemos el `List.forEach`, como versión funcional del bucle `for`. Emplea el `forEach` para recorrer una lista de `Integer` y que muestre un mensaje si el entero es positivo.

El método forEach es una forma funcional de recorrer colecciones en Java. En lugar de escribir explícitamente un bucle for, se delega el recorrido de los elementos en la propia colección y se proporciona una función que se ejecutará para cada elemento. Esto hace el código más declarativo y más cercano al estilo funcional, ya que se especifica qué hacer con cada elemento, pero no cómo recorrer la estructura internamente.

El método forEach recibe como parámetro una interfaz funcional Consumer<T>, es decir, una función que recibe un elemento y no devuelve ningún valor. Habitualmente se utiliza junto con funciones lambda para expresar de forma compacta la operación que se desea realizar sobre cada elemento de la colección.

import java.util.List;

public class Main {

    public static void main(String[] args) {

        List<Integer> numeros =
                List.of(3, -2, 7, 0, -5, 10);

        numeros.forEach(n -> {
            if (n > 0) {
                System.out.println(
                    n + " es positivo"
                );
            }
        });
    }
}

## 15. Repasando el tema de genericidad, fíjate en la firma de `forEach`, ¿por qué se usa `Consumer<? super T>` y no `Consumer<T>`? Explica qué significa **PECS**, y explícalo para el caso de mejorar el ejemplo del método `transformar` la hora de definir el tipo de la función transformadora.

El método forEach utiliza Consumer<? super T> en lugar de Consumer<T> para hacer el código más flexible mediante contravarianza. Un Consumer consume objetos de tipo T, es decir, recibe elementos pero no los produce. Si una colección contiene elementos de tipo Integer, resulta perfectamente válido utilizar un Consumer<Integer>, pero también uno de Number o incluso uno de Object, ya que todos ellos son capaces de consumir Integer. Por ello se utiliza ? super T, permitiendo aceptar consumidores de tipos más generales.

Esta idea se resume en la regla conocida como PECS, siglas de “Producer Extends, Consumer Super”. La regla indica que, cuando una estructura produce valores de tipo T para ser leídos, debe utilizarse ? extends T. En cambio, cuando una estructura consume valores de tipo T que se van a insertar o procesar, debe utilizarse ? super T. Esto permite aprovechar la covarianza y contravarianza de forma segura en los genéricos de Java.

En el caso del método transformar, la función transformadora consume un valor de entrada y produce un valor de salida. Por tanto, siguiendo PECS, el parámetro de entrada de la función puede expresarse mediante ? super T, porque consume objetos de tipo T, mientras que el tipo de salida puede expresarse mediante ? extends R, porque produce valores compatibles con R. De este modo, el método acepta funciones más generales y reutilizables sin perder seguridad de tipos.

## 16. Referencias a métodos. Podemos obtener una referencia a métodos de objetos o clases. Pon un ejemplo en JavaScript y en Java, de una clase `Persona` con un método `saludar`. En el código principal, crea una `Persona` con un nombre, y obtén una referencia a su método `saludar` en una variable local. Invoca `saludar` con esa referencia a su método `saludar`.

Las referencias a métodos permiten tratar métodos existentes como valores reutilizables, de forma similar a las funciones lambda. En lugar de definir una nueva función, se reutiliza directamente un método ya existente de un objeto o de una clase. Esto resulta útil cuando el comportamiento deseado ya está implementado y simplemente se quiere pasar o almacenar una referencia a dicho comportamiento.

En JavaScript, los métodos de los objetos pueden almacenarse en variables y ejecutarse posteriormente. Sin embargo, debe tenerse cuidado con el contexto asociado al objeto, ya que el valor de this puede perderse si el método se separa del objeto original. En Java, las referencias a métodos se integran dentro del sistema de interfaces funcionales y permiten reutilizar métodos existentes mediante una sintaxis más compacta que las lambdas equivalentes.

class Persona {
    constructor(nombre) {
        this.nombre = nombre;
    }

    saludar() {
        console.log("Hola, soy " + this.nombre);
    }
}

const persona = new Persona("Carlos");

const referenciaSaludo =
    persona.saludar.bind(persona);

referenciaSaludo();

import java.util.function.Consumer;

class Persona {
    private final String nombre;

    public Persona(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Hola, soy " + nombre);
    }
}

public class Main {

    public static void main(String[] args) {

        Persona persona = new Persona("Carlos");

        Runnable referenciaSaludo =
                persona::saludar;

        referenciaSaludo.run();
    }
}

## 17. ¿Qué tipos de referencias a método se pueden hacer en Java? Pon un ejemplo de referencia a método estático, a constructor, a método de instancia de una instancia concreta y a método de instancia sobre cualquier instancia.

En Java existen varios tipos de referencias a método, todas ellas utilizadas para reutilizar métodos ya existentes de forma compacta y compatible con interfaces funcionales. Las referencias a método son una alternativa más breve y expresiva a ciertas funciones lambda cuando la lambda simplemente invoca un método ya definido. Dependiendo del tipo de método al que se haga referencia, la sintaxis y el comportamiento cambian ligeramente.

Uno de los tipos es la referencia a método estático, donde se referencia un método perteneciente a una clase. También existen referencias a constructores, que permiten crear objetos utilizando una sintaxis funcional. Otro caso es la referencia a método de instancia de una instancia concreta, donde el método se asocia a un objeto específico ya existente. Finalmente, existe la referencia a método de instancia sobre cualquier instancia de un tipo, donde el objeto sobre el que se ejecutará el método se recibe implícitamente como parámetro.

import java.util.function.Function;
import java.util.function.Supplier;
import java.util.function.Consumer;

class Persona {

    private final String nombre;

    public Persona(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Hola, soy " + nombre);
    }

    public static String aMayusculas(String texto) {
        return texto.toUpperCase();
    }
}

public class Main {

    public static void main(String[] args) {

        // Referencia a método estático
        Function<String, String> f1 =
                Persona::aMayusculas;

        // Referencia a constructor
        Function<String, Persona> f2 =
                Persona::new;

        // Referencia a método de instancia
        // de una instancia concreta
        Persona p = new Persona("Carlos");

        Runnable f3 = p::saludar;

        // Referencia a método de instancia
        // sobre cualquier instancia
        Consumer<Persona> f4 =
                Persona::saludar;
    }
}

## 18. Otro ejemplo expresivo. Ordena una lista de `Persona`, cada persona tiene un nombre y una edad (de tipo entero). Ordena la lista de `Persona` con `Collections.sort`, pasándole como comparador una expresión lambda que compare la edad de ambas personas y si tienen la misma edad, se ordene por orden alfabético del nombre. Crea dos versiones: Una con la función de comparación hecha manualmente, y otra empleando `Comparator`.

Ese código muestra correctamente los cuatro tipos principales de referencias a método en Java.
La referencia:
Function<String, String> f1 =        Persona::aMayusculas;
es una referencia a método estático. No necesita una instancia de Persona, ya que el método pertenece directamente a la clase. La función recibe un String y devuelve otro String.
La referencia:
Function<String, Persona> f2 =        Persona::new;
es una referencia a constructor. Permite crear objetos de Persona de forma funcional. Cuando se invoque apply sobre esa función, se ejecutará el constructor correspondiente.
La referencia:
Runnable f3 = p::saludar;
es una referencia a método de instancia de una instancia concreta. El objeto p ya existe y la referencia queda asociada específicamente a ese objeto. Al ejecutar run, se llamará a saludar sobre esa instancia concreta.
Finalmente:
Consumer<Persona> f4 =        Persona::saludar;
es una referencia a método de instancia sobre cualquier instancia. Aquí no se fija un objeto concreto. La instancia sobre la que se ejecutará el método se recibe implícitamente como parámetro del Consumer. Por tanto, cuando se invoque accept pasando una Persona, se ejecutará saludar sobre esa instancia recibida.