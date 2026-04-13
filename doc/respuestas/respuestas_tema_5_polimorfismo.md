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

El polimorfismo es un mecanismo de la programación orientada a objetos que permite tratar objetos de distintos tipos como si fueran del mismo tipo base, haciendo que respondan de forma diferente a una misma operación. Es decir, se puede invocar un mismo método sobre distintos objetos y que cada uno ejecute su propia implementación. Esto resulta útil para escribir código más general y flexible, ya que permite trabajar con colecciones heterogéneas de objetos sin necesidad de conocer su tipo concreto.

Gracias al polimorfismo, el comportamiento concreto se decide en tiempo de ejecución en función del tipo real del objeto, no del tipo de la referencia. Esto permite extender el sistema añadiendo nuevas clases sin modificar el código existente que utiliza el tipo base. En la práctica, es una herramienta fundamental para reducir el acoplamiento y facilitar la extensibilidad del software.

La sobreescritura de métodos consiste en que una subclase proporciona su propia implementación de un método que ya existe en la superclase. Para que esto ocurra, el método debe tener la misma firma, y la versión de la subclase sustituye a la de la superclase cuando se invoca sobre un objeto de ese tipo. Esto permite especializar o modificar el comportamiento heredado sin cambiar la definición original en la clase base.

## 2. ¿En qué consiste la **"ligadura dinámica"** o **"enlace tardío"**? ¿qué relación tiene con el polimorfismo? ¿hay que indicarlos explícitamente al programar o depende esto del lenguaje? Compara C++ y Java. Indicalo después también para Python.

La ligadura dinámica o enlace tardío es el mecanismo por el cual la llamada a un método se resuelve en tiempo de ejecución, en lugar de en tiempo de compilación. Esto significa que, cuando se invoca un método a través de una referencia de tipo general, no se decide en ese momento qué implementación concreta se ejecutará, sino que se determina en función del tipo real del objeto en memoria. De este modo, el comportamiento final depende del objeto concreto y no únicamente del tipo de la referencia.

Este concepto está directamente relacionado con el polimorfismo. El polimorfismo permite tratar distintos objetos como si fueran del mismo tipo base, y la ligadura dinámica es lo que hace posible que cada objeto ejecute su propia versión del método. Sin enlace tardío, todas las llamadas se resolverían de forma estática y no se podría obtener comportamiento diferente según el tipo real del objeto. Por tanto, la ligadura dinámica es el mecanismo que permite que el polimorfismo funcione en la práctica.

En C++, la ligadura dinámica no es automática en todos los casos. Para que un método se resuelva dinámicamente, debe declararse como virtual en la clase base. Si no se hace, el enlace es estático y se decide en tiempo de compilación. En Java, en cambio, la ligadura dinámica es el comportamiento por defecto para los métodos de instancia, por lo que no es necesario indicar nada especial para que se produzca. Esto simplifica el uso del polimorfismo, ya que no requiere decisiones explícitas por parte del programador en cada método.

En Python, el enlace tardío también es el comportamiento natural del lenguaje. Al tratarse de un lenguaje dinámico, las llamadas a métodos se resuelven siempre en tiempo de ejecución en función del objeto real. No existe la necesidad de declarar métodos como virtuales ni de especificar ningún mecanismo adicional. Esto hace que el polimorfismo sea aún más flexible, aunque también implica menos control en comparación con lenguajes como C++.

## 3. Pon un ejemplo sencillo en Java, de un `Soldado`, con un método `saluda`, con dos subclases: `Zapador` y `Artillero`, donde `Zapador` sobreescribe el método `saludar`, sustituyendo por completo su comportamiento. Ilustra el funcionamiento del polimorfismo creando un array de `Soldados` de dos tipos y luego recorriéndolo empleando referencias de tipo `Soldado` y llamando a `saludar`.

class Soldado {
    private String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Hola, soy el soldado " + nombre);
    }
}

class Zapador extends Soldado {
    public Zapador(String nombre) {
        super(nombre);
    }

    @Override
    public void saludar() {
        System.out.println("Soy un zapador y me encargo de las minas");
    }
}

class Artillero extends Soldado {
    public Artillero(String nombre) {
        super(nombre);
    }
}

public class Main {
    public static void main(String[] args) {
        Soldado[] ejercito = new Soldado[3];

        ejercito[0] = new Soldado("Juan");
        ejercito[1] = new Zapador("Luis");
        ejercito[2] = new Artillero("Carlos");

        for (Soldado s : ejercito) {
            s.saludar();
        }
    }
}

## 4. Si sobreescribo un método, ¿puedo invocar el método base para trabajar a partir de su resultado? Haz que zapador cambie ligeramente la forma de saludar, que salude de forma normal, tal cual hace el soldado base, pero que además añada un "ZAPADOR A SUS ORDENES" ¿qué palabra clave del lenguaje has usado para invocar al método de la clase base?

Cuando se sobreescribe un método en una subclase, no es obligatorio reemplazar completamente su comportamiento, sino que también es posible reutilizar la implementación de la clase base y extenderla. Esto permite construir nuevas funcionalidades a partir del comportamiento ya definido, en lugar de duplicarlo o reescribirlo desde cero. Es una práctica habitual cuando se desea mantener parte del comportamiento original y añadir detalles específicos en la subclase.

Para invocar el método de la clase base desde la subclase se utiliza la palabra clave super. Esta palabra permite acceder a los métodos y constructores de la superclase, incluso cuando han sido sobreescritos. De este modo, la subclase puede ejecutar primero la lógica definida en la clase base y después añadir su propio comportamiento adicional.

En el caso del zapador, se puede llamar al método saludar de la clase base y, a continuación, añadir un mensaje propio. Así se consigue modificar ligeramente el comportamiento heredado sin perder la funcionalidad original:

class Soldado {
    private String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Hola, soy el soldado " + nombre);
    }
}

class Zapador extends Soldado {
    public Zapador(String nombre) {
        super(nombre);
    }

    @Override
    public void saludar() {
        super.saludar();
        System.out.println("ZAPADOR A SUS ORDENES");
    }
}

## 5. Al sobreescribir un método en Java, ¿qué restricciones existen sobre los tipos de los parámetros y el tipo de retorno? ¿Qué diferencia hay entre sobreescritura (*overriding*) y sobrecarga (*overloading*)? ¿Para qué sirve la anotación `@Override` y por qué es recomendable usarla siempre?

Al sobreescribir un método en Java, deben respetarse ciertas restricciones para que el compilador lo considere una redefinición válida. El método de la subclase debe tener exactamente el mismo nombre y los mismos tipos de parámetros que el método de la superclase. En cuanto al tipo de retorno, debe ser el mismo o un subtipo compatible del original, lo que se conoce como retorno covariante. Además, no se puede reducir la visibilidad del método heredado, es decir, no se puede pasar de public a protected o private.

La diferencia entre sobreescritura y sobrecarga es fundamental. La sobreescritura ocurre entre una clase base y una subclase, y consiste en redefinir un método ya existente para modificar su comportamiento. En cambio, la sobrecarga se produce dentro de una misma clase cuando existen varios métodos con el mismo nombre pero distintos parámetros. En este último caso, el compilador decide qué método utilizar en función de los argumentos, mientras que en la sobreescritura la decisión se toma en tiempo de ejecución según el tipo real del objeto.

La anotación @Override se utiliza para indicar explícitamente que un método está sobrescribiendo otro de la superclase. No es obligatoria, pero es muy recomendable porque permite al compilador verificar que la sobreescritura es correcta. Si existe algún error, como un nombre mal escrito o una firma incorrecta, el compilador lo detectará. Esto ayuda a evitar fallos sutiles y mejora la claridad del código, ya que deja claro que se está redefiniendo un comportamiento heredado.

## 6. Entonces, cuando se estudia Java, ¿se emplea el polimorfismo desde el principio? Por ejemplo, sobreescribiendo `toString` o sobreescribiendo `equals`, ¿ya estoy usando polimorfismo?

Sí, en Java se está utilizando polimorfismo desde el principio, incluso aunque no se sea plenamente consciente de ello. Muchas de las primeras clases que se escriben ya heredan de la clase base Object, y al redefinir métodos como toString o equals se está modificando el comportamiento que tendrán los objetos cuando se utilicen a través de referencias generales. Esto significa que, aunque no se trabaje explícitamente con jerarquías complejas, el mecanismo del polimorfismo ya está presente.

Cuando se sobreescribe toString o equals, se está aplicando polimorfismo porque el comportamiento que se ejecuta depende del tipo real del objeto. Por ejemplo, al imprimir un objeto, el sistema invoca el método toString, pero la versión que se ejecuta será la definida en la clase concreta del objeto, no la de la clase base. Lo mismo ocurre con equals, donde cada clase puede definir qué significa que dos objetos sean iguales, y esa lógica se aplica en tiempo de ejecución.

Por tanto, sí se está utilizando polimorfismo desde etapas muy tempranas en Java. Aunque no se esté trabajando todavía con múltiples subclases complejas, el simple hecho de redefinir métodos heredados y que el sistema seleccione automáticamente la implementación adecuada en función del objeto real ya es una manifestación directa del polimorfismo.

## 7. ¿Qué es una **"clase abstracta"**? ¿Qué es un **"método abstracto"**? ¿Puedo crear instancias de una clase abstracta? Pongamos un ejemplo en Java: Redefinamos `Soldado`, hagamos que, además del método `saluda` que ya tenía, tenga un método `atacar`, que sea abstracto y que cada tipo de soldado haga su acción cuando se le pida atacar. ¿Donde debemos poner `abstract`?

Una clase abstracta es una clase que no está pensada para ser instanciada directamente, sino para servir como base de otras clases. Se utiliza cuando se quiere definir una estructura común y compartir comportamiento entre varias subclases, pero dejando algunos aspectos sin implementar para que cada subclase los defina según sus necesidades. De esta forma, se establece un contrato parcial que obliga a las clases derivadas a completar ciertos métodos.

Un método abstracto es un método que se declara en la clase base sin proporcionar una implementación. Solo se especifica su firma, y se obliga a las subclases a implementarlo. Este tipo de métodos se utiliza cuando se sabe que todas las subclases deben tener ese comportamiento, pero no tiene sentido definirlo de forma general en la clase base. Una clase que contiene al menos un método abstracto debe declararse también como abstracta.

No se pueden crear instancias de una clase abstracta, ya que su definición está incompleta. Solo se pueden crear objetos de sus subclases concretas, que proporcionan las implementaciones necesarias. En el ejemplo, la clase Soldado se declara como abstracta y contiene un método abstracto atacar, mientras que cada tipo de soldado define cómo realiza esa acción. La palabra clave abstract se utiliza tanto en la declaración de la clase como en la del método que no tiene implementación.

## 8. ¿Qué efecto tiene la palabra clave `final` sobre métodos y clases en Java? ¿Cómo se relaciona con el polimorfismo? ¿Conoces algún ejemplo de clase `final` en la propia API estándar de Java?

La palabra clave final en Java se puede aplicar a métodos y a clases con distintos efectos. Cuando se aplica a un método, indica que ese método no puede ser sobreescrito por ninguna subclase. Esto significa que el comportamiento definido en ese método queda fijado y no puede ser modificado en clases derivadas. Cuando se aplica a una clase, indica que esa clase no puede ser extendida, es decir, no puede tener subclases.

En relación con el polimorfismo, final limita su uso. El polimorfismo se basa en la posibilidad de que distintas subclases redefinan métodos y proporcionen comportamientos distintos. Si un método es final, se impide su sobreescritura y, por tanto, no puede participar en ese comportamiento polimórfico. De forma similar, si una clase es final, no puede formar parte de una jerarquía de herencia, lo que impide aplicar polimorfismo basado en subclases sobre ella.

Un ejemplo conocido de clase final en la API estándar de Java es String. Esta clase no puede ser extendida, lo que garantiza que su comportamiento no pueda ser alterado mediante herencia. Esto es importante para mantener la inmutabilidad y la seguridad de las cadenas de texto en el lenguaje.

## 9. En Java, qué son las **"interfaces"**? ¿Son como clases abstractas? ¿Una clase puede implementar más de una interfaz?

Las interfaces en Java son un mecanismo que permite definir un conjunto de métodos que una clase debe implementar, sin proporcionar una implementación concreta de esos métodos. Funcionan como un contrato que especifica qué operaciones debe ofrecer una clase, pero no cómo deben llevarse a cabo. De esta forma, se separa completamente la definición del comportamiento de su implementación, lo que facilita el diseño modular y flexible.

Aunque las interfaces comparten ciertas similitudes con las clases abstractas, no son exactamente lo mismo. Una clase abstracta puede contener tanto métodos abstractos como métodos con implementación, además de atributos de instancia. En cambio, una interfaz se centra en definir métodos que deben ser implementados por las clases, y tradicionalmente no contiene estado interno como una clase normal. Por tanto, una interfaz es una forma más pura de abstracción, mientras que una clase abstracta permite combinar abstracción y código reutilizable.

Una clase puede implementar más de una interfaz en Java. Esto permite que una misma clase cumpla varios contratos distintos, proporcionando una forma de herencia múltiple de comportamiento sin los problemas asociados a la herencia múltiple de clases. Gracias a esto, se pueden diseñar sistemas más flexibles donde una clase puede participar en distintos roles o contextos sin necesidad de pertenecer a múltiples jerarquías de herencia.

## 10. Vamos a poner un ejemplo nuevo con polimorfismo. Queremos implementar una clase `Punto`, con un método `calcularDistanciaA`, que permite calcular la distancia a otro `Punto`. Sin embargo, como queremos trabajar con puntos 2D y 3D, haz que ese método sea abstracto y haya dos implementaciones de ese cálculo de distancia. Emplea `instanceof` y *downcasting* para verificar que se recibe un punto compatible y poder calcular correctamente la distancia siempre entre puntos del mismo subtipo. Aprovecha este diseño para crear ahora una clase `Linea`, que acepta `Punto`, sin saber de qué tipo es, y es capaz de dar su longitud independientemente de las dimensiones de sus puntos (las cuales desconoce).

Se puede plantear una jerarquía en la que Punto sea una clase abstracta que defina el contrato común de todos los puntos, dejando abstracto el método calcularDistanciaA. De ese modo, cada subtipo concreto se responsabiliza de implementar el cálculo de distancia según sus propias dimensiones. Esta solución permite que una clase como Linea trabaje con referencias del tipo general Punto sin necesidad de conocer si está tratando con puntos en dos o en tres dimensiones.

Para asegurar que la distancia solo se calcule entre puntos compatibles, cada implementación concreta puede comprobar con instanceof si el punto recibido pertenece al mismo subtipo. Si no ocurre así, puede lanzarse una excepción indicando que no se pueden mezclar dimensiones distintas. Una vez comprobado el tipo real del parámetro, se realiza un downcasting para poder acceder a los atributos específicos del subtipo correspondiente y efectuar el cálculo correcto. Así, el control de compatibilidad queda encapsulado dentro de cada clase concreta.

Este diseño también permite que la clase Linea sea completamente polimórfica. Linea solo necesita almacenar dos referencias de tipo Punto y delegar en el método calcularDistanciaA el cálculo de su longitud. Por tanto, Linea no necesita saber si internamente trabaja con puntos 2D o 3D, ya que el comportamiento adecuado se resolverá dinámicamente en función del tipo real de los objetos. De esta forma se reutiliza la misma clase Linea para distintas representaciones geométricas sin modificar su código.

abstract class Punto {
    public abstract double calcularDistanciaA(Punto otro);
}

class Punto2D extends Punto {
    private final double x;
    private final double y;

    public Punto2D(double x, double y) {
        this.x = x;
        this.y = y;
    }

    @Override
    public double calcularDistanciaA(Punto otro) {
        if (!(otro instanceof Punto2D)) {
            throw new IllegalArgumentException(
                "No se puede calcular distancia entre un Punto2D y un punto de otro subtipo."
            );
        }

        Punto2D p = (Punto2D) otro;
        double dx = this.x - p.x;
        double dy = this.y - p.y;
        return Math.sqrt(dx * dx + dy * dy);
    }
}

class Punto3D extends Punto {
    private final double x;
    private final double y;
    private final double z;

    public Punto3D(double x, double y, double z) {
        this.x = x;
        this.y = y;
        this.z = z;
    }

    @Override
    public double calcularDistanciaA(Punto otro) {
        if (!(otro instanceof Punto3D)) {
            throw new IllegalArgumentException(
                "No se puede calcular distancia entre un Punto3D y un punto de otro subtipo."
            );
        }

        Punto3D p = (Punto3D) otro;
        double dx = this.x - p.x;
        double dy = this.y - p.y;
        double dz = this.z - p.z;
        return Math.sqrt(dx * dx + dy * dy + dz * dz);
    }
}

class Linea {
    private final Punto p1;
    private final Punto p2;

    public Linea(Punto p1, Punto p2) {
        if (p1 == null || p2 == null) {
            throw new IllegalArgumentException("Los puntos de la línea no pueden ser nulos.");
        }
        this.p1 = p1;
        this.p2 = p2;
    }

    public double longitud() {
        return p1.calcularDistanciaA(p2);
    }
}

public class Main {
    public static void main(String[] args) {
        Linea linea2D = new Linea(new Punto2D(0, 0), new Punto2D(3, 4));
        System.out.println("Longitud línea 2D: " + linea2D.longitud());

        Linea linea3D = new Linea(new Punto3D(0, 0, 0), new Punto3D(1, 2, 2));
        System.out.println("Longitud línea 3D: " + linea3D.longitud());
    }
}

## 11. ¿Qué es la **"herencia de interfaces"** en Java? ¿Existe **"herencia múltiple de interfaces"**? Pon un ejemplo de una interfaz `Fichero` que tenga un método para leer su contenido en forma de `String` y luego dicha interfaz sea extendida por otra que sea `FicheroEscribible` que permita enviar contenido e incluso eliminar el fichero.

La herencia de interfaces en Java consiste en que una interfaz puede extender otra interfaz, heredando todos sus métodos. Esto permite construir jerarquías de interfaces donde se define primero un conjunto básico de operaciones y, posteriormente, se amplía con nuevas funcionalidades en interfaces más específicas. De este modo, se pueden establecer contratos progresivos, donde una interfaz más especializada incluye todo lo definido en la anterior más nuevos métodos.

En Java sí existe herencia múltiple de interfaces. Una interfaz puede extender varias interfaces al mismo tiempo, lo que permite combinar distintos contratos en uno solo sin los problemas de ambigüedad que aparecen en la herencia múltiple de clases. Esto es posible porque las interfaces definen comportamiento sin estado interno, lo que evita conflictos en la implementación. Por tanto, una clase puede implementar una interfaz que a su vez hereda de varias, cumpliendo así múltiples contratos simultáneamente.

A continuación se muestra un ejemplo donde se define una interfaz Fichero con un método para leer su contenido, y otra interfaz FicheroEscribible que la extiende añadiendo métodos para escribir y eliminar el contenido:

interface Fichero {
    String leer();
}

interface FicheroEscribible extends Fichero {
    void escribir(String contenido);
    void eliminar();
}