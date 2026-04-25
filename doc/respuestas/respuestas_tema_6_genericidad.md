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

En lenguajes como C o Java, es posible construir estructuras de datos genéricas utilizando mecanismos que permitan almacenar referencias a cualquier tipo. En C se utiliza habitualmente el tipo void*, que actúa como un puntero genérico capaz de apuntar a cualquier dato, aunque sin información de tipo. En Java, este papel lo desempeña la clase Object, que es la superclase de todos los objetos, permitiendo almacenar cualquier instancia en una misma estructura.

Este tipo de diseño permite crear estructuras como listas o arrays capaces de contener elementos heterogéneos. Sin embargo, implica una pérdida de información de tipo, ya que al recuperar los elementos es necesario realizar conversiones explícitas para poder utilizarlos correctamente. Esto introduce riesgos en tiempo de ejecución si se realizan conversiones incorrectas, por lo que este enfoque requiere cuidado adicional por parte del programador.

A continuación se muestra un ejemplo sencillo en Java de una estructura basada en un array de Object que permite almacenar cualquier tipo de dato:

class Contenedor {
    private Object[] datos;
    private int size;

    public Contenedor(int capacidad) {
        datos = new Object[capacidad];
        size = 0;
    }

    public void add(Object elemento) {
        if (size >= datos.length) {
            throw new IllegalStateException("Contenedor lleno");
        }
        datos[size++] = elemento;
    }

    public Object get(int index) {
        if (index < 0 || index >= size) {
            throw new IndexOutOfBoundsException();
        }
        return datos[index];
    }

    public int size() {
        return size;
    }
}

public class Main {
    public static void main(String[] args) {
        Contenedor c = new Contenedor(5);

        c.add("Hola");
        c.add(123);
        c.add(3.14);

        System.out.println(c.get(0));
        System.out.println(c.get(1));
        System.out.println(c.get(2));
    }
}

## 2. Brevemente, ¿Qué significa la **programación genérica**? ¿Es el ejemplo anterior un ejemplo básico de programación genérica? 

La programación genérica es una técnica que permite definir clases, métodos o estructuras de datos que funcionan con distintos tipos de datos sin necesidad de duplicar código. En lugar de trabajar con un tipo concreto, se utilizan parámetros de tipo que se especifican posteriormente al usar la clase o método. Esto permite mantener la seguridad de tipos en tiempo de compilación y evita la necesidad de conversiones explícitas, haciendo el código más seguro y reutilizable.

El ejemplo anterior no es un ejemplo real de programación genérica, sino una aproximación básica basada en el uso de Object. Aunque permite almacenar cualquier tipo de dato, pierde la información de tipo y obliga a realizar conversiones manuales al recuperar los elementos. Esto puede provocar errores en tiempo de ejecución si se realizan conversiones incorrectas.

La programación genérica, tal y como se implementa en Java con genéricos, soluciona estos problemas al mantener la información de tipo durante la compilación. De este modo, el compilador puede comprobar que los tipos son correctos y evitar errores antes de ejecutar el programa. Por tanto, el ejemplo anterior ilustra una forma primitiva de generalización, pero no aprovecha las ventajas completas de la programación genérica.

## 3. Indica los problemas respecto al chequeo de tipos, de emplear `void*` o `Object` cuando se crean estructuras de datos genéricas. 

El principal problema de utilizar void* en C o Object en Java es la pérdida de información de tipo en tiempo de compilación. Al almacenar todos los elementos como un tipo genérico, el compilador ya no puede comprobar si los datos que se insertan o se recuperan son del tipo esperado. Esto implica que muchos errores relacionados con tipos no se detectan durante la compilación, sino que aparecen en tiempo de ejecución, lo que dificulta su diagnóstico y corrección.

Otro inconveniente es la necesidad de realizar conversiones explícitas al recuperar los datos. Como la estructura no conoce el tipo real de los elementos almacenados, el programador debe indicar manualmente el tipo esperado. Si esta conversión no es correcta, pueden producirse errores en ejecución, como accesos inválidos en C o excepciones de tipo en Java. Este proceso introduce código adicional y aumenta la probabilidad de errores.

Además, el uso de estos mecanismos reduce la claridad y expresividad del código. No queda claro qué tipo de datos se espera manejar en cada estructura, lo que dificulta su comprensión y mantenimiento. En contraste, la programación genérica permite mantener la información de tipo, asegurando un control más estricto en tiempo de compilación y evitando la mayoría de estos problemas.

## 4. Vamos entonces con mecanismos de mejora de la programación genérica ¿Qué son los **parámetros de tipo**? 

Los parámetros de tipo son una característica de la programación genérica que permiten definir clases, métodos o estructuras de datos de forma independiente del tipo concreto con el que van a trabajar. En lugar de fijar un tipo específico, se introduce un identificador que representa un tipo genérico, el cual se concreta posteriormente cuando se utiliza la clase o el método. De este modo, se puede reutilizar el mismo código para distintos tipos manteniendo un diseño flexible.

A diferencia del uso de Object o void*, los parámetros de tipo conservan la información de tipo durante la compilación. Esto permite que el compilador realice comprobaciones de tipos antes de ejecutar el programa, evitando errores comunes derivados de conversiones incorrectas. Además, elimina la necesidad de realizar castings explícitos al recuperar los datos, lo que simplifica el código y lo hace más seguro.

En Java, los parámetros de tipo se expresan mediante una notación como <T>, donde T representa un tipo genérico que será sustituido por un tipo concreto al crear una instancia o invocar un método. Esto permite definir estructuras de datos y algoritmos reutilizables, manteniendo tanto la generalidad como la seguridad de tipos, que eran difíciles de conseguir con enfoques anteriores basados en tipos genéricos no tipados.


## 5. En Java existe "generics", en C++ existen "templates". Pon un ejemplo de uso de programación genérica en ambos, instanciando una lista o vector dinámico que solo admite `String`. Introduce valores, y luego haz un recorrido de ellos mostrando cómo cada elemento es del tipo concreto con seguridad.

En programación genérica, tanto en Java como en C++, se pueden definir estructuras de datos que trabajan con un tipo concreto especificado en el momento de su uso. Esto permite que el compilador verifique que solo se insertan elementos del tipo adecuado, evitando errores de tipo en tiempo de ejecución. Además, al recuperar los elementos, no es necesario realizar conversiones explícitas, ya que el tipo está garantizado.

En Java, esto se consigue mediante generics. Por ejemplo, se puede utilizar una lista dinámica que solo admita objetos de tipo String. El compilador impide insertar cualquier otro tipo, y al recorrer la lista, cada elemento ya es tratado directamente como String con total seguridad:

import java.util.ArrayList;
import java.util.List;

public class Main {
    public static void main(String[] args) {
        List<String> lista = new ArrayList<>();

        lista.add("Hola");
        lista.add("Mundo");
        lista.add("Java");

        for (String s : lista) {
            System.out.println(s.toUpperCase());
        }
    }
}

En C++, el mismo concepto se implementa mediante templates. En este caso, se puede utilizar un vector que solo admita objetos de tipo std::string. Al igual que en Java, el compilador garantiza que todos los elementos son del tipo correcto y permite trabajar con ellos sin conversiones adicionales:

#include <iostream>
#include <vector>
#include <string>

int main() {
    std::vector<std::string> lista;

    lista.push_back("Hola");
    lista.push_back("Mundo");
    lista.push_back("C++");

    for (const std::string& s : lista) {
        std::cout << s << std::endl;
    }

    return 0;
}

## 6. Sobre el funcionamiento de la programación genérica. ¿Qué hace el compilador cuando se instancia una clase que tiene parámetros de tipo? ¿Hace lo mismo C++ y Java? ¿Qué es el "type erasure" de Java y la "instanciación de plantillas" de C++?

Cuando se instancia una clase con parámetros de tipo, el compilador utiliza esa información para comprobar que los tipos utilizados son correctos. Esto permite detectar errores en tiempo de compilación, como intentar insertar un tipo incorrecto en una estructura genérica. Además, el compilador adapta internamente el código para que funcione con el tipo concreto especificado, aunque el modo en que lo hace depende del lenguaje.

En Java, el compilador aplica un mecanismo llamado type erasure. Esto significa que la información de los parámetros de tipo se utiliza durante la compilación para verificar los tipos, pero se elimina en el código generado. En tiempo de ejecución, todos los objetos genéricos se tratan como si fueran de tipo Object o de su límite superior si lo tienen. Por ello, el código compilado no contiene información sobre los tipos genéricos, y el compilador introduce conversiones automáticas cuando es necesario.

En C++, el funcionamiento es distinto. Se utiliza la instanciación de plantillas, que consiste en generar versiones concretas del código para cada tipo utilizado. Es decir, cuando se usa un template con un tipo determinado, el compilador crea una versión específica de la clase o función para ese tipo. Esto implica que el código resultante contiene implementaciones separadas para cada tipo, manteniendo la información de tipo también en tiempo de ejecución.

Por tanto, la principal diferencia es que Java elimina la información de tipo tras la compilación, mientras que C++ genera código específico para cada tipo. Esto tiene implicaciones en rendimiento, uso de memoria y capacidad de reflexión sobre tipos, siendo cada enfoque adecuado para distintos objetivos dentro del diseño del lenguaje.

## 7. Vamos a crear una nueva clase con parámetros de tipo. Define en Java una clase `Par`, que permite alojar dos valores de tipos diferentes. Incluye un constructor y un getter para cada tipo. Pon un ejemplo de uso de ese `Par`, por ejemplo para especificar el tipo de retorno de una función que devuelve en un `Par` la media y desviación típica de un array de `double`. 

Se puede definir una clase con parámetros de tipo para representar un par de valores potencialmente distintos sin perder la seguridad de tipos. Para ello se utilizan dos parámetros de tipo, por ejemplo T y U, que permiten que cada posición del par tenga un tipo diferente. La clase solo define su estructura y comportamiento, mientras que los tipos concretos se especifican al crear instancias.

Este diseño evita el uso de Object y elimina la necesidad de conversiones explícitas, ya que el compilador garantiza que cada valor es del tipo correcto. Además, permite reutilizar la misma clase para múltiples combinaciones de tipos, como cadenas, números u otros objetos. El acceso a los valores se realiza mediante métodos getter, que devuelven directamente el tipo correspondiente.

A continuación se muestra una posible implementación de la clase Par, junto con un ejemplo de uso en el que se devuelve la media y la desviación típica de un array de valores:

class Par<T, U> {
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

public class Main {
    public static Par<Double, Double> calcularMediaYDesviacion(double[] datos) {
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
        double[] valores = {2.0, 4.0, 6.0, 8.0};

        Par<Double, Double> resultado = calcularMediaYDesviacion(valores);

        System.out.println("Media: " + resultado.getPrimero());
        System.out.println("Desviación típica: " + resultado.getSegundo());
    }
}

## 8. En Java, se pueden declarar parámetros de tipo también a nivel de método, no solo a nivel de clase. Pon un ejemplo con un método genérico `seleccionaUno`, que pasados dos objetos del mismo tipo, te devuelva aleatoriamente uno de ellos. Muestra la diferencia de definirlo con dos `Object`, a definirlo con dos parámetros de tipo, en terminos de (i) evitar downcasting y (ii) forzar que ambos objetos sean del mismo tipo. 

En Java, los parámetros de tipo también pueden declararse a nivel de método, lo que permite definir operaciones genéricas sin necesidad de parametrizar toda la clase. Esto resulta útil cuando solo un método necesita trabajar de forma genérica. Un método genérico puede aceptar parámetros de un tipo abstracto que se concreta en el momento de la llamada, manteniendo así la seguridad de tipos y evitando problemas asociados al uso de Object.
Si se define el método utilizando Object, se pierde la información de tipo. Esto implica que al recuperar el valor devuelto será necesario realizar una conversión explícita, lo que puede provocar errores en tiempo de ejecución si no se realiza correctamente. Además, el compilador no puede garantizar que ambos argumentos sean del mismo tipo, por lo que podrían pasarse objetos incompatibles sin que se detecte el error en compilación.
En cambio, si se utiliza un parámetro de tipo, el compilador asegura que ambos argumentos son del mismo tipo y que el valor devuelto también lo es. Esto elimina la necesidad de realizar conversiones explícitas y evita errores de tipo. De este modo, se mejora la seguridad y la claridad del código, ya que el tipo se mantiene consistente durante toda la operación.

// Versión con Object
public static Object seleccionaUno(Object a, Object b) {
    if (Math.random() < 0.5) {
        return a;
    } else {
        return b;
    }
}

// Uso (requiere casting)
String resultado = (String) seleccionaUno("Hola", "Mundo");

## 9. ¿Se pueden establecer restricciones en los parámetros de tipo? Por ejemplo, si quiero definir un tipo genérico `<T>`, ¿puedo decir que tenga que ser, al menos, un número para poder tratarlo como tal? Pon un ejemplo en Java de un `Punto` con dos coordenadas, metodos `getX`, `getY`, y una función `calcularDistanciaA` otro `Punto`. Permite que esas coordenadas sean cualquier tipo de número. Pon dos soluciones: una simplemente creando coordenadas de tipo `Number` y otra añadiendo generics para reforzar el chequeo de tipos y saber exactamente con qué tipo de número trabaja el `Punto`. En este caso y respecto al "type erasure", ¿cuál es el tipo final tras la compilación?

Sí, en Java se pueden establecer restricciones sobre los parámetros de tipo mediante límites. Para indicar que un tipo genérico debe ser, como mínimo, un número, se puede escribir T extends Number. Esto significa que T debe ser Number o alguna subclase suya, como Integer, Double, Float o Long. Así, el código puede tratar los valores como números y utilizar métodos comunes como doubleValue.

Una primera solución consiste en declarar directamente las coordenadas como Number. Esto permite almacenar cualquier tipo numérico, pero no garantiza que las dos coordenadas sean del mismo tipo concreto. Por ejemplo, podría mezclarse un Integer con un Double dentro del mismo punto. Además, al consultar las coordenadas, solo se sabe que son Number, no el tipo numérico concreto original.

class PuntoNumber {
    private final Number x;
    private final Number y;

    public PuntoNumber(Number x, Number y) {
        this.x = x;
        this.y = y;
    }

    public Number getX() {
        return x;
    }

    public Number getY() {
        return y;
    }

    public double calcularDistanciaA(PuntoNumber otro) {
        double dx = this.x.doubleValue() - otro.x.doubleValue();
        double dy = this.y.doubleValue() - otro.y.doubleValue();
        return Math.sqrt(dx * dx + dy * dy);
    }
}

La segunda solución consiste en usar genéricos con una restricción T extends Number. De este modo, el punto sigue pudiendo trabajar con distintos tipos numéricos, pero cada instancia queda asociada a un tipo concreto. Por ejemplo, un Punto<Double> trabaja con coordenadas Double y un Punto<Integer> trabaja con coordenadas Integer. Esto refuerza el chequeo de tipos, ya que evita mezclar de forma accidental puntos parametrizados con tipos distintos.

class Punto<T extends Number> {
    private final T x;
    private final T y;

    public Punto(T x, T y) {
        this.x = x;
        this.y = y;
    }

    public T getX() {
        return x;
    }

    public T getY() {
        return y;
    }

    public double calcularDistanciaA(Punto<T> otro) {
        double dx = this.x.doubleValue() - otro.x.doubleValue();
        double dy = this.y.doubleValue() - otro.y.doubleValue();
        return Math.sqrt(dx * dx + dy * dy);
    }
}

## 10. Sobre las soluciones anteriores. Si bien ambas permiten trabajar con distintos tipos de número sin duplicar la clase `Punto`, reflexiona sobre el refuerzo del chequeo de tipos con generics. ¿Permiten ambas crear un punto con una coordenada de tipo entero y la otra coordenada de tipo real? ¿Qué tipo devuelve el `getX` con la solucion sin generics y qué tipo devuelve el que tiene la solución con generics?

Ambas soluciones permiten reutilizar la misma clase para distintos tipos numéricos, pero no ofrecen el mismo nivel de control sobre los tipos. En la solución basada únicamente en Number, no existe ninguna restricción que impida mezclar tipos distintos en una misma instancia. Por tanto, es posible crear un punto donde una coordenada sea un entero y la otra un número real, ya que ambos son compatibles con Number. En cambio, en la solución con genéricos, el tipo T es único para toda la instancia, por lo que ambas coordenadas deben ser del mismo tipo concreto. Esto evita mezclar tipos de forma accidental.

Esta diferencia supone un refuerzo importante del chequeo de tipos en tiempo de compilación. Con generics, el compilador detecta errores si se intenta combinar, por ejemplo, un Integer y un Double en el mismo punto parametrizado. En la versión sin generics, ese tipo de mezcla es válida desde el punto de vista del compilador, lo que puede dar lugar a comportamientos menos claros o inconsistentes en el uso de la clase.

En cuanto al tipo devuelto por los métodos de acceso, también hay una diferencia relevante. En la solución sin generics, el método getX devuelve un valor de tipo Number, lo que obliga a tratar el resultado de forma genérica o a realizar conversiones si se necesita un tipo concreto. En la solución con generics, el método getX devuelve un valor de tipo T, es decir, el tipo concreto con el que se ha parametrizado la clase. Esto proporciona mayor precisión y evita la necesidad de conversiones adicionales, mejorando tanto la seguridad como la claridad del código.

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

public interface Punto<T> {
    double distanciaA(T p);
}

public class Punto2D implements Punto<Punto2D> {
    private final double x;
    private final double y;

    public Punto2D(double x, double y) {
        this.x = x;
        this.y = y;
    }

    @Override
    public double distanciaA(Punto2D p) {
        return Math.sqrt(
            Math.pow(this.x - p.x, 2) +
            Math.pow(this.y - p.y, 2)
        );
    }
}

public class Punto3D implements Punto<Punto3D> {
    private final double x;
    private final double y;
    private final double z;

    public Punto3D(double x, double y, double z) {
        this.x = x;
        this.y = y;
        this.z = z;
    }

    @Override
    public double distanciaA(Punto3D p) {
        return Math.sqrt(
            Math.pow(this.x - p.x, 2) +
            Math.pow(this.y - p.y, 2) +
            Math.pow(this.z - p.z, 2)
        );
    }
}

public class Main {
    public static void main(String[] args) {
        Punto2D a = new Punto2D(0, 0);
        Punto2D b = new Punto2D(3, 4);

        System.out.println(a.distanciaA(b));

        Punto3D c = new Punto3D(0, 0, 0);
        Punto3D d = new Punto3D(1, 2, 2);

        System.out.println(c.distanciaA(d));

        // Esto no compila:
        // a.distanciaA(c);
    }
}

## 12. Dado que `String` es subtipo de `Object`, ¿significa eso que `List<String>` es subtipo de `List<Object>`? ¿Y que `String[]` es subtipo de `Object[]`? Razona por qué la respuesta es diferente en cada caso y qué problema en tiempo de ejecución puede aparecer con los arrays. A partir de estos ejemplos, define qué significa que un tipo genérico sea **covariante**, **contravariante** o **invariante** respecto a su parámetro de tipo.

No, en Java no se cumple que List<String> sea subtipo de List<Object>, aunque String sí sea subtipo de Object. Los tipos genéricos en Java son invariantes, lo que significa que List<String> y List<Object> son tipos completamente distintos y no existe relación de subtipado entre ellos. Esto se debe a que permitir esa conversión rompería la seguridad de tipos, ya que se podrían insertar objetos que no son String en una lista que debería contener solo cadenas.

En cambio, con los arrays sí ocurre que String[] es subtipo de Object[]. Los arrays en Java son covariantes, lo que permite esa asignación. Sin embargo, esto introduce un problema en tiempo de ejecución. Si se trata un String[] como si fuera un Object[] y se intenta almacenar un objeto que no sea String, como un Integer, el compilador lo permite, pero en ejecución se lanza una excepción de tipo ArrayStoreException. Esto muestra que la covarianza de arrays no es completamente segura.

A partir de estos ejemplos, se pueden definir tres conceptos. Un tipo es covariante cuando se preserva la relación de subtipado, como ocurre con los arrays en Java. Es contravariante cuando la relación se invierte, es decir, un tipo más general puede sustituir a uno más específico en ciertas posiciones, aunque este caso no aparece directamente en los ejemplos anteriores. Un tipo es invariante cuando no existe ninguna relación de subtipado entre versiones parametrizadas con distintos tipos, como ocurre con los genéricos en Java. Esta invariancia se adopta precisamente para evitar errores en tiempo de ejecución y garantizar la seguridad de tipos en compilación.

## 13. Java permite recuperar covarianza y contravarianza en tipos genéricos de forma controlada mediante **wildcards**. ¿Qué es un wildcard (`?`)? Muestra la diferencia entre `List<? extends T>` y `List<? super T>`, indicando en qué casos se usa cada uno. Pon dos ejemplos: (i) un método que reciba una lista de números y calcule su suma, usando `? extends`; (ii) un método que reciba una lista y le añada varios números enteros, usando `? super`.

Un wildcard es un mecanismo de los genéricos en Java que permite trabajar con tipos desconocidos de forma flexible. Se representa con el símbolo ?, y se utiliza cuando no se necesita conocer el tipo exacto del parámetro genérico, sino solo ciertas propiedades de él. Permite expresar relaciones de covarianza y contravarianza de forma controlada, evitando los problemas de seguridad de tipos que aparecerían si los genéricos fueran directamente covariantes.

La forma List<? extends T> representa covarianza. Indica que la lista contiene elementos de algún subtipo de T, aunque no se sepa exactamente cuál. Esto permite leer elementos como tipo T con seguridad, pero no permite añadir nuevos elementos (salvo null), ya que no se conoce el tipo concreto. En cambio, List<? super T> representa contravarianza. Indica que la lista contiene elementos de algún supertipo de T. En este caso sí es seguro añadir elementos de tipo T, pero al leer solo se puede tratar como Object, ya que no se conoce el tipo más específico almacenado.

A continuación se muestran dos ejemplos que ilustran estos usos:

// (i) Lectura: suma de números usando ? extends
public static double sumarLista(List<? extends Number> lista) {
    double suma = 0;
    for (Number n : lista) {
        suma += n.doubleValue();
    }
    return suma;
}

// (ii) Escritura: añadir enteros usando ? super
public static void añadirEnteros(List<? super Integer> lista) {
    lista.add(10);
    lista.add(20);
    lista.add(30);
}