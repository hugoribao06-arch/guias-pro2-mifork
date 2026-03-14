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

En C es posible construir estructuras más complejas a partir de otras más simples mediante composición. Esta técnica consiste en incluir una estructura dentro de otra como uno de sus campos, estableciendo una relación del tipo “A tiene un B”. De esta manera se pueden modelar entidades más complejas reutilizando estructuras previamente definidas y agrupando datos relacionados en una sola unidad lógica.

Para representar una línea formada por dos puntos, primero se define una estructura que describa un punto en el plano mediante sus coordenadas x e y. Posteriormente se define una segunda estructura que represente una línea, la cual contiene dos puntos como campos. La relación de composición se establece porque la línea contiene dos instancias de la estructura que representa los puntos.

Además de las estructuras, se pueden definir funciones que operen sobre estos tipos. Una función puede calcular la distancia entre dos puntos aplicando la fórmula de distancia euclídea. Otra función puede calcular la longitud de una línea reutilizando la función anterior, ya que una línea está formada precisamente por dos puntos.

#include <math.h>

struct Punto {
    double x;
    double y;
};

struct Linea {
    struct Punto p1;
    struct Punto p2;
};

double distancia(struct Punto a, struct Punto b) {
    double dx = a.x - b.x;
    double dy = a.y - b.y;
    return sqrt(dx * dx + dy * dy);
}

double longitudLinea(struct Linea l) {
    return distancia(l.p1, l.p2);
}

## 2. Ahora transforma ese ejemplo a orientación a objetos con Java, para tener un primer ejemplo de **composición** en orientación a objetos. Crea una clase `Punto`, y una clase `Linea`. La clase `Punto` debe tener un método para calcular distancia a otro `Punto` y `Linea` debe tener un método para calcular su longitud. Gracias a la ocultación de información, supera a C, garantizando que los puntos sean inmutables, al igual que la línea, que una vez creada, no queremos que se modifique de qué a qué puntos va dicha línea.  

En Java, la composición se expresa mediante la inclusión de objetos de una clase como atributos de otra. En este caso, una línea puede modelarse como un objeto que contiene dos puntos, estableciendo la relación conceptual “una línea tiene dos puntos”. A diferencia de C, el lenguaje proporciona mecanismos de encapsulación y ocultación de información, lo que permite controlar cómo se crean y utilizan los objetos.

La clase Punto puede definirse con atributos privados que representen las coordenadas. Si estos atributos se declaran final y no se proporcionan métodos para modificarlos, el objeto se vuelve inmutable: una vez creado, sus coordenadas no pueden cambiar. Esta propiedad garantiza que cualquier cálculo realizado con un punto siempre se base en el mismo estado interno. Además, la clase puede incluir un método para calcular la distancia a otro punto utilizando la fórmula de distancia euclídea.

La clase Linea se construye mediante composición, ya que contiene dos objetos Punto como atributos privados. Si dichos atributos también se declaran final y no se ofrecen métodos que permitan modificarlos, se asegura que la línea sea igualmente inmutable. De esta forma, una vez creada la línea, no es posible cambiar los puntos que la definen. La clase puede incluir un método que calcule su longitud delegando el cálculo en el método de distancia de la clase Punto.

public class Punto {
    private final double x;
    private final double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double distancia(Punto otro) {
        double dx = this.x - otro.x;
        double dy = this.y - otro.y;
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

    public double longitud() {
        return p1.distancia(p2);
    }
}

## 3. ¿Qué significa la **multiplicidad** en la composición? En el ejemplo anterior, ¿cuál es la multiplicidad entre `Linea` y `Punto`? Indícalo expresando la multiplicidad en ambas direcciones, de `Linea` a `Punto` y de `Punto` a `Linea`.

La multiplicidad en una relación de composición indica cuántas instancias de una clase pueden estar asociadas con una instancia de otra clase. En otras palabras, especifica el número mínimo y máximo de objetos que participan en la relación. Este concepto se utiliza habitualmente en el modelado de sistemas orientados a objetos, por ejemplo en diagramas de clases, para expresar restricciones estructurales entre los objetos que forman parte de una composición.

En una composición, la multiplicidad describe tanto cuántos elementos contiene el objeto compuesto como cuántos objetos compuestos pueden estar relacionados con un mismo componente. Esta información se expresa normalmente en ambos extremos de la relación, ya que cada dirección describe una restricción distinta. La multiplicidad permite, por tanto, entender con precisión cómo se organiza la estructura de los objetos dentro de un sistema.

En el ejemplo considerado, la multiplicidad de `Linea` a `Punto`** es 2, ya que cada línea está formada exactamente por dos puntos. Por otra parte, la multiplicidad de `Punto` a `Linea`** puede considerarse 0.., ya que un punto puede no pertenecer a ninguna línea o puede formar parte de múltiples líneas distintas dentro de un programa.

## 4. ¿Qué significa composición **fuerte** y composición **débil**? ¿Qué consecuencia implica en relación al ciclo de vida de los objetos? Indica a cuál solemos referirnos como **"asociación o agregación"** y a cuál como **"composición"** propiamente.

La composición fuerte y la composición débil describen dos formas de relación entre objetos según el grado de dependencia entre ellos. Ambas representan relaciones del tipo “un objeto contiene a otros”, pero se diferencian en cómo se gestionan y en qué medida los objetos contenidos dependen del objeto que los agrupa.

La composición fuerte implica una dependencia completa entre el objeto contenedor y los objetos contenidos. Los componentes forman parte esencial del objeto compuesto y su ciclo de vida está ligado al del objeto principal. Esto significa que los componentes se crean como parte del objeto contenedor y dejan de existir cuando dicho objeto se destruye. En este caso, los componentes no tienen sentido de forma independiente fuera del objeto que los contiene. En modelado orientado a objetos, esta relación se denomina normalmente composición en sentido estricto.

La composición débil, en cambio, describe una relación en la que los objetos relacionados pueden existir de forma independiente. El objeto contenedor mantiene referencias a otros objetos, pero no controla necesariamente su ciclo de vida. Los objetos asociados pueden haber sido creados fuera de la estructura y pueden seguir existiendo incluso si el objeto que los referencia desaparece. Esta forma de relación suele denominarse asociación o agregación, dependiendo del nivel de relación estructural que se quiera expresar.

## 5. Cuando una clase usa a otra al recibirla o devolverla como parámetro en algún método, al hacer `new` dentro de un método, o al usarlas como variables locales, ¿hablamos de composición o de **"dependencia"**?

Cuando una clase utiliza otra únicamente dentro de sus métodos, por ejemplo recibiéndola como parámetro, devolviéndola como resultado, creando instancias con `new` dentro de un método o empleándola como variable local, no se considera una relación de composición. En estos casos se habla de una relación de dependencia entre clases.

La dependencia indica que una clase necesita a otra para poder implementar alguna parte de su comportamiento, pero esa relación no forma parte de su estructura interna. Es decir, la clase no contiene objetos de la otra como atributos permanentes, sino que simplemente los utiliza temporalmente durante la ejecución de algunos métodos.

Por tanto, la relación descrita corresponde a una dependencia. La composición, en cambio, aparece cuando una clase incluye objetos de otra como parte de su estado interno, normalmente mediante atributos, estableciendo una relación estructural más estable entre ambas clases.

## 6. En el ejemplo anterior de línea y punto, programa la relación entre `Linea` y `Punto` de dos formas. Una **como composición fuerte**, donde el ciclo de vida de los puntos está ligado al de Linea y otra **como composición débil**, donde no.

En una composición fuerte, el objeto compuesto es responsable de crear y gestionar completamente los objetos que forman parte de él. Los componentes no se reciben desde fuera, sino que se crean internamente y su existencia depende del objeto principal. Esto implica que el ciclo de vida de los componentes está ligado al del objeto que los contiene, ya que no se comparten con otras estructuras ni se utilizan de manera independiente.

En el caso de una línea formada por dos puntos, una composición fuerte puede implementarse haciendo que la clase Linea cree internamente los objetos Punto. De esta forma, los puntos nacen como parte de la línea y no se reutilizan fuera de ella. La línea controla completamente su creación y uso.

class Punto {
    private final double x;
    private final double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double distancia(Punto otro) {
        double dx = this.x - otro.x;
        double dy = this.y - otro.y;
        return Math.sqrt(dx * dx + dy * dy);
    }
}

class Linea {
    private final Punto p1;
    private final Punto p2;

    public Linea(double x1, double y1, double x2, double y2) {
        this.p1 = new Punto(x1, y1);
        this.p2 = new Punto(x2, y2);
    }

    public double longitud() {
        return p1.distancia(p2);
    }
}

En una composición débil, los objetos que forman parte de la relación pueden existir de manera independiente. La clase que los utiliza no es necesariamente responsable de su creación y puede recibirlos desde el exterior. Esto permite que los mismos objetos puedan utilizarse en distintas estructuras y que su ciclo de vida no dependa del objeto que los contiene.

class Punto {
    private final double x;
    private final double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double distancia(Punto otro) {
        double dx = this.x - otro.x;
        double dy = this.y - otro.y;
        return Math.sqrt(dx * dx + dy * dy);
    }
}

class Linea {
    private final Punto p1;
    private final Punto p2;

    public Linea(Punto p1, Punto p2) {
        this.p1 = p1;
        this.p2 = p2;
    }

    public double longitud() {
        return p1.distancia(p2);
    }
}

## 7. En Java, en la composición fuerte, ¿cuando el contenedor destruye los objetos? No se observa que `Linea` destruya los `Punto` explícitamente, ¿Por qué?

En Java no existe destrucción explícita de objetos como ocurre en lenguajes como C o C++. La memoria se gestiona automáticamente mediante un sistema de recolección de basura. Por esta razón, una clase no necesita liberar manualmente los objetos que contiene ni llamar a ninguna operación de destrucción cuando deja de utilizarlos.

En una composición fuerte, se considera que el ciclo de vida de los objetos componentes está ligado al del objeto contenedor desde el punto de vista lógico del diseño. Sin embargo, en Java esa relación no se implementa mediante llamadas explícitas de destrucción, sino mediante referencias entre objetos. Mientras el objeto contenedor exista y mantenga referencias a sus componentes, estos permanecerán accesibles.

Cuando el objeto contenedor deja de existir o deja de ser accesible desde el programa, las referencias que mantenía a sus componentes desaparecen también. Si no existen otras referencias hacia esos objetos, el recolector de basura podrá liberar la memoria asociada a ellos en algún momento posterior. Por ello no es necesario que la clase `Linea` destruya explícitamente los objetos `Punto`.

## 8. Pon un ejemplo de composicion débil entre un departamento que tiene varios profesores. Implementa dos composiciones a la vez: entre el departamento y todos sus profesores y entre el departamento y su director, que es un profesor del departamento. Siempre debe haber un director en el departamento desde el inicio. Lanza excepciones si se viola la invariante. Emplea arrays primitivos de Java, estilo `Profesor[]`, con máximo 50, pero no rompas la encapsulación, no desveles que estás empleando un array, permite añadir un `Profesor` al final de la lista, y eliminar un profesor dada su posición. Da acceso a los profesores con un método para saber cuántos hay y otro para obtener un profesor por posición. El director se puede cambiar por otro profesor del departamento. Sin embargo, ten en cuenta esta invariante de clase: el director debe formar siempre parte de la lista de profesores, es decir, ten cuidado al cambiar el director o al eliminar un profesor.

Este caso representa una composición débil porque los objetos Profesor pueden existir independientemente del objeto Departamento. El departamento mantiene referencias a sus profesores y a su director, pero no necesariamente controla su ciclo de vida completo. Además, aparece una segunda relación: el director también es un profesor del propio departamento, por lo que debe cumplirse de forma permanente la invariante de que el director pertenezca siempre a la lista de profesores.

Para mantener esa invariante, el constructor debe exigir desde el principio un director válido e incorporarlo a la lista de profesores. A partir de ahí, cualquier operación que pueda romper la consistencia debe comprobarse y, si es necesario, lanzar una excepción. En particular, no debe permitirse nombrar como director a un profesor que no pertenezca al departamento, ni eliminar de la lista al profesor que en ese momento actúa como director.

Aunque internamente se utilice un array Profesor[] de tamaño máximo 50, no debe exponerse directamente, para no romper la encapsulación. Por eso se ofrecen métodos para añadir profesores, eliminar por posición, consultar cuántos hay y recuperar un profesor por posición. Desde fuera, el almacenamiento interno queda oculto y solo se observa la interfaz pública de la clase.

class Profesor {
    private final String nombre;

    public Profesor(String nombre) {
        if (nombre == null || nombre.isEmpty()) {
            throw new IllegalArgumentException("El nombre del profesor no puede ser nulo ni vacío.");
        }
        this.nombre = nombre;
    }

    public String getNombre() {
        return nombre;
    }
}

class Departamento {
    private static final int MAX_PROFESORES = 50;

    private final Profesor[] profesores;
    private int numProfesores;
    private Profesor director;

    public Departamento(Profesor director) {
        if (director == null) {
            throw new IllegalArgumentException("El departamento debe crearse con un director.");
        }

        this.profesores = new Profesor[MAX_PROFESORES];
        this.numProfesores = 0;

        this.profesores[0] = director;
        this.numProfesores = 1;
        this.director = director;
    }

    public int getNumProfesores() {
        return numProfesores;
    }

    public Profesor getProfesor(int posicion) {
        comprobarPosicion(posicion);
        return profesores[posicion];
    }

    public Profesor getDirector() {
        return director;
    }

    public void addProfesor(Profesor profesor) {
        if (profesor == null) {
            throw new IllegalArgumentException("No se puede añadir un profesor nulo.");
        }
        if (numProfesores >= MAX_PROFESORES) {
            throw new IllegalStateException("No caben más profesores en el departamento.");
        }

        profesores[numProfesores] = profesor;
        numProfesores++;
    }

    public void setDirector(Profesor nuevoDirector) {
        if (nuevoDirector == null) {
            throw new IllegalArgumentException("El director no puede ser nulo.");
        }
        if (!contieneProfesor(nuevoDirector)) {
            throw new IllegalArgumentException("El director debe pertenecer al departamento.");
        }

        this.director = nuevoDirector;
    }

    public void removeProfesor(int posicion) {
        comprobarPosicion(posicion);

        if (profesores[posicion] == director) {
            throw new IllegalStateException("No se puede eliminar al director del departamento.");
        }

        for (int i = posicion; i < numProfesores - 1; i++) {
            profesores[i] = profesores[i + 1];
        }

        profesores[numProfesores - 1] = null;
        numProfesores--;
    }

    private void comprobarPosicion(int posicion) {
        if (posicion < 0 || posicion >= numProfesores) {
            throw new IndexOutOfBoundsException("Posición fuera de rango.");
        }
    }

    private boolean contieneProfesor(Profesor profesor) {
        for (int i = 0; i < numProfesores; i++) {
            if (profesores[i] == profesor) {
                return true;
            }
        }
        return false;
    }
}

## 9. En Java, existen también `List`, cambia y muestra cómo sería el código anterior empleando `List` en vez de arrays primitivos. ¿Qué parte del código original te has ahorrado? Además, fíjate en el método `getProfesor(int pos)`: si en su lugar existiera un método que devolviera todos los profesores a la vez, ¿qué problema tendría devolver directamente la lista interna? ¿Cómo lo resolverías?

Al sustituir el array Profesor[] por una List<Profesor>, la implementación se simplifica porque deja de ser necesario gestionar manualmente el tamaño ocupado, el desplazamiento de elementos al borrar, el límite fijo del array y varias comprobaciones asociadas al almacenamiento interno. La lista ya proporciona operaciones para añadir al final, acceder por posición, eliminar por posición y conocer cuántos elementos contiene. Por tanto, se mantiene la misma idea de encapsulación, pero con una estructura más flexible y con menos código auxiliar.

La parte principal que se elimina respecto a la versión con arrays es la relacionada con la gestión manual del almacenamiento: ya no hace falta el atributo numProfesores, tampoco el bucle que desplaza elementos al eliminar, ni la asignación a null de la última posición libre, ni la comprobación de capacidad máxima del array. Sigue siendo necesario, sin embargo, comprobar la validez lógica de las operaciones para no romper la invariante de clase, en especial que el director pertenezca siempre al departamento y que no se elimine al profesor que actúa como director.

Si existiera un método que devolviera todos los profesores a la vez, devolver directamente la lista interna rompería la encapsulación. Desde fuera podría modificarse esa lista sin pasar por los controles de la clase, por ejemplo eliminando al director o añadiendo valores no válidos, lo que permitiría violar la invariante. Para evitarlo, debe devolverse una vista no modificable de la lista o bien una copia independiente. Así se sigue dando acceso de lectura a los profesores sin exponer la estructura interna real del objeto.

import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

class Profesor {
    private final String nombre;

    public Profesor(String nombre) {
        if (nombre == null || nombre.isEmpty()) {
            throw new IllegalArgumentException("El nombre del profesor no puede ser nulo ni vacío.");
        }
        this.nombre = nombre;
    }

    public String getNombre() {
        return nombre;
    }
}

import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

class Departamento {
    private final List<Profesor> profesores;
    private Profesor director;

    public Departamento(Profesor director) {
        if (director == null) {
            throw new IllegalArgumentException("El departamento debe crearse con un director.");
        }

        this.profesores = new ArrayList<>();
        this.profesores.add(director);
        this.director = director;
    }

    public int getNumProfesores() {
        return profesores.size();
    }

    public Profesor getProfesor(int posicion) {
        return profesores.get(posicion);
    }

    public Profesor getDirector() {
        return director;
    }

    public void addProfesor(Profesor profesor) {
        if (profesor == null) {
            throw new IllegalArgumentException("No se puede añadir un profesor nulo.");
        }

        profesores.add(profesor);
    }

    public void setDirector(Profesor nuevoDirector) {
        if (nuevoDirector == null) {
            throw new IllegalArgumentException("El director no puede ser nulo.");
        }
        if (!profesores.contains(nuevoDirector)) {
            throw new IllegalArgumentException("El director debe pertenecer al departamento.");
        }

        this.director = nuevoDirector;
    }

    public void removeProfesor(int posicion) {
        Profesor profesorAEliminar = profesores.get(posicion);

        if (profesorAEliminar == director) {
            throw new IllegalStateException("No se puede eliminar al director del departamento.");
        }

        profesores.remove(posicion);
    }

    public List<Profesor> getProfesores() {
        return Collections.unmodifiableList(profesores);
    }
}

## 10. Al igual que ocurre con las excepciones en Java, que pueden encerrar causas (que son excepciones), de forma recursiva, suponen un tipo especial de composiciones, denominadas composiciones recursivas. Pon un ejemplo en Java de una `Persona`, que sea inmutable, y que tiene una madre, que es otra `Persona`. Haz un main con un ejemplo de uso con una familia de personas, desde el nieto hasta la abuela. Enumera algún otro ejemplo clásico de composiciones recursivas.

Una composición recursiva ocurre cuando una clase contiene referencias a objetos de su mismo tipo. Esto permite representar estructuras en las que un elemento está relacionado con otro de la misma clase. En el ejemplo planteado, una persona tiene una madre que también es una persona, por lo que la relación se repite de forma potencialmente indefinida a lo largo de las generaciones. Este tipo de diseño permite modelar estructuras jerárquicas o encadenadas.

Para garantizar la inmutabilidad, los atributos de la clase se declaran final y no se proporcionan métodos que permitan modificarlos tras la construcción del objeto. De esta forma, una vez creada una persona, su nombre y su madre quedan fijados permanentemente. La madre puede ser otra instancia de Persona o bien null en el caso de que no se conozca o no se quiera representar una generación anterior.

class Persona {
    private final String nombre;
    private final Persona madre;

    public Persona(String nombre, Persona madre) {
        if (nombre == null || nombre.isEmpty()) {
            throw new IllegalArgumentException("El nombre no puede ser nulo ni vacío.");
        }
        this.nombre = nombre;
        this.madre = madre;
    }

    public String getNombre() {
        return nombre;
    }

    public Persona getMadre() {
        return madre;
    }
}

El siguiente programa muestra un ejemplo de uso con tres generaciones: una abuela, su hija y un nieto. Cada persona se construye indicando su madre, lo que crea una cadena de relaciones entre objetos del mismo tipo.

public class Main {
    public static void main(String[] args) {
        Persona abuela = new Persona("Carmen", null);
        Persona madre = new Persona("Laura", abuela);
        Persona nieto = new Persona("Diego", madre);

        System.out.println("Nieto: " + nieto.getNombre());
        System.out.println("Madre del nieto: " + nieto.getMadre().getNombre());
        System.out.println("Abuela del nieto: " + nieto.getMadre().getMadre().getNombre());
    }
}

## 11. ¿Qué son las relaciones de composición "bidireccionales"? ¿Qué habría que hacer para implementar este tipo de relación en el ejemplo de `Profesor` y `Departamento`?

Las relaciones de composición bidireccionales son aquellas en las que ambas clases implicadas mantienen una referencia a la otra. Esto significa que no solo un objeto conoce o contiene al otro, sino que también el segundo objeto mantiene una referencia de vuelta al primero. De esta forma, la relación puede recorrerse en ambas direcciones, permitiendo navegar desde cualquiera de los dos objetos hacia el otro.

En el ejemplo anterior, la relación existente es unidireccional, ya que el `Departamento` conoce a sus `Profesor`, pero los objetos `Profesor` no tienen ninguna referencia al departamento al que pertenecen. Para convertir esta relación en bidireccional, sería necesario añadir en la clase `Profesor` un atributo que referencie al `Departamento` al que está asociado cada profesor. De este modo, un profesor podría conocer el departamento al que pertenece.

Además de añadir la referencia, sería necesario mantener la consistencia entre ambos lados de la relación. Cuando se añade un profesor al departamento, habría que actualizar también el atributo correspondiente dentro del profesor para que apunte a ese departamento. De forma similar, cuando un profesor se elimina del departamento, también habría que actualizar su referencia para reflejar que ya no pertenece a él. Mantener sincronizadas ambas direcciones es esencial para que el modelo de objetos no entre en un estado incoherente.
