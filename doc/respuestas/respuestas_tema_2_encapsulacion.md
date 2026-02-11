<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Encapsulación". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 2. Encapsulación

## 1. En Programación Orientada a Objetos (POO), ¿Qué buscan la **encapsulación** y **la ocultación** de información? Enumera brevemente algunas ventajas de la ocultación de información.

En Programación Orientada a Objetos, la **encapsulación** y la **ocultación de información** buscan separar claramente la interfaz pública de una clase de su implementación interna. La encapsulación agrupa datos y comportamiento dentro de una misma unidad (la clase), mientras que la ocultación impide el acceso directo a ciertos detalles internos, obligando a interactuar con el objeto únicamente a través de métodos definidos. De esta manera, se controla cómo se modifican los datos y se protege la coherencia del estado del objeto.

La ocultación de información permite que los detalles internos puedan cambiar sin afectar al código que utiliza la clase. Es decir, mientras la interfaz pública se mantenga estable, la implementación puede modificarse libremente. Esto facilita la evolución del software y reduce el impacto de cambios internos en otras partes del programa.

Entre las ventajas principales de la ocultación de información se pueden enumerar:

1. Mayor seguridad y control sobre los datos, evitando modificaciones indebidas.
2. Reducción del acoplamiento entre módulos, lo que mejora la mantenibilidad.
3. Mayor claridad en el diseño, al definir explícitamente qué se puede usar desde el exterior.
4. Facilita la detección de errores, ya que se limita el acceso directo a los atributos internos.

## 2. ¿Qué se entiende por la **interfaz pública** de un objeto o clase en POO? Describe brevemente cómo se relaciona con la ocultación de información.

La interfaz pública de una clase u objeto es el conjunto de métodos y miembros que pueden ser utilizados desde fuera de la propia clase. Representa la parte visible del tipo, es decir, aquello que otros módulos del programa pueden invocar o consultar. En Java, esta interfaz está formada principalmente por los métodos declarados como públicos, que definen qué operaciones están permitidas sobre los objetos de esa clase.

La interfaz pública actúa como un contrato entre la clase y el resto del programa. Indica qué se puede hacer con el objeto, pero no cómo está implementado internamente. De esta manera, el código que utiliza la clase no necesita conocer los detalles internos de almacenamiento o funcionamiento, sino únicamente cómo invocar sus métodos correctamente.

La relación con la ocultación de información es directa. La ocultación consiste en mantener los detalles internos fuera del alcance del exterior, limitando el acceso únicamente a través de la interfaz pública. Así, los atributos suelen declararse como privados y solo se manipulan mediante métodos públicos. Esto permite proteger el estado interno del objeto y modificar su implementación sin afectar al código que depende de su interfaz.

## 3. Brevemente: ¿Por qué hay que ser conscientes y diseñar con cuidado la **interfaz pública** de una clase? ¿Es fácil cambiarla?

La interfaz pública de una clase debe diseñarse con cuidado porque define la forma en que otras partes del programa interactuarán con ella. Una vez que una clase es utilizada por distintos módulos, su interfaz se convierte en un contrato estable. Si se diseña de manera poco clara o se exponen más métodos de los necesarios, se incrementa el acoplamiento y se dificulta el mantenimiento del sistema.

No es sencillo cambiar la interfaz pública cuando ya está en uso. Cualquier modificación en la firma de un método público, en sus parámetros o en su comportamiento esperado puede obligar a modificar todo el código que depende de esa clase. En proyectos grandes, este impacto puede ser considerable y generar errores difíciles de detectar.

Por ello, resulta recomendable exponer únicamente lo imprescindible, mantener una interfaz coherente y bien documentada, y evitar cambios innecesarios una vez que la clase forma parte del diseño estable del sistema. Un diseño cuidadoso reduce problemas futuros y facilita la evolución del software.

## 4. ¿Qué son las **invariantes de clase** y por qué la ocultación de información nos ayuda?

Las invariantes de clase son condiciones o reglas que deben cumplirse siempre para que un objeto se considere en un estado válido. Se trata de propiedades internas que deben mantenerse verdaderas después de la construcción del objeto y tras la ejecución de cualquiera de sus métodos públicos. Por ejemplo, una clase que represente una cuenta bancaria podría tener como invariante que el saldo nunca sea negativo, si así lo establece su diseño.

Estas invariantes forman parte del diseño lógico de la clase, aunque no siempre estén escritas explícitamente en el código. Cada método público debe garantizar que, tras su ejecución, el objeto siga cumpliendo dichas condiciones. De esta forma, se preserva la coherencia interna y se evita que el objeto quede en un estado inconsistente.

La ocultación de información ayuda directamente a mantener las invariantes, ya que impide que el código externo modifique los atributos internos de manera directa. Si los datos están protegidos y solo pueden alterarse mediante métodos controlados, resulta más sencillo asegurar que cualquier cambio respete las reglas definidas. Sin ocultación, cualquier parte del programa podría romper las invariantes sin control, comprometiendo la fiabilidad del sistema.

## 5. Pon un ejemplo de una clase `Punto` en `Java`, con dos coordenadas, `x` e `y`, de tipo `double`, con un método `calcularDistanciaAOrigen`, y que haga uso de la ocultación de información. ¿Cuál es la interfaz pública de la clase `Punto`? ¿Qué significa `public` y `private`?

A continuación se muestra una posible definición de la clase Punto aplicando ocultación de información. Las coordenadas no son accesibles directamente desde fuera de la clase y solo pueden consultarse mediante métodos públicos.

public class Punto {
    private double x;
    private double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double calcularDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }

    public double getX() {
        return x;
    }

    public double getY() {
        return y;
    }
}

La interfaz pública de la clase Punto está formada por el constructor público, el método calcularDistanciaAOrigen y los métodos getX y getY. Son los únicos elementos accesibles desde fuera y constituyen el contrato que la clase ofrece al resto del programa. Todo lo demás queda oculto como parte de su implementación interna.

El modificador public indica que un miembro es accesible desde cualquier otra clase. En cambio, private restringe el acceso exclusivamente al interior de la propia clase. Esta distinción es la base de la ocultación de información, ya que permite separar claramente qué forma parte de la interfaz visible y qué pertenece a los detalles internos de implementación.

## 6. En Java, ¿A quiénes se pueden aplicar los modificadores `public` o `private`?

En Java, los modificadores public y private se pueden aplicar a distintos elementos del programa para controlar su visibilidad. Principalmente se utilizan en atributos, métodos y constructores dentro de una clase. De esta manera, se decide qué partes forman parte de la interfaz accesible desde el exterior y cuáles quedan restringidas al uso interno de la propia clase.

En el caso de los atributos, declarar un campo como private impide que otras clases accedan directamente a él, reforzando la encapsulación. Cuando se aplica a métodos o constructores, se controla si pueden ser invocados desde fuera de la clase o solo desde su interior. Esto permite diseñar clases con una interfaz pública clara y una implementación interna protegida.

También pueden aplicarse a clases, aunque con ciertas restricciones. Una clase declarada como public puede ser utilizada desde cualquier otro paquete, mientras que una clase sin modificador explícito tiene visibilidad por defecto (limitada al mismo paquete). Sin embargo, las clases internas sí pueden declararse como private, restringiendo su uso al interior de la clase que las contiene.

## 7. En POO, la visibilidad puede ser pública o privada, pero ¿existen más tipos de visibilidad? ¿Qué ocurre en Java? ¿Y en otros lenguajes?

En programación orientada a objetos no solo existen las visibilidades pública y privada. Muchos lenguajes incorporan niveles intermedios que permiten un control más preciso sobre quién puede acceder a los miembros de una clase. Estos niveles adicionales se utilizan para equilibrar encapsulación y reutilización, especialmente cuando se trabaja con herencia o con organización por módulos o paquetes.

En Java existen cuatro niveles de visibilidad. Public permite el acceso desde cualquier parte del programa. Private restringe el acceso exclusivamente a la propia clase. Protected permite el acceso desde la misma clase, desde clases del mismo paquete y desde subclases, incluso si están en otros paquetes. Además, existe la visibilidad por defecto, también llamada package-private, que se aplica cuando no se especifica ningún modificador y permite el acceso solo desde clases del mismo paquete.

En otros lenguajes pueden existir variantes similares o adicionales. En C++ existen public, private y protected, pero no existe el concepto de paquete como en Java. En C# también se incluyen modificadores como internal, que restringe el acceso al mismo ensamblado. Por tanto, aunque los conceptos básicos son comunes, cada lenguaje implementa el control de visibilidad con matices propios según su modelo de organización del código.

## 8. Responde: Los miembros de instancia privados de un objeto están ocultos para (a) otras clases o (b) otras instancias, aunque sean de la misma clase. Pon un ejemplo añadiendo un método `calcularDistanciaAPunto(Punto otro)` y explica la respuesta.

Los miembros de instancia declarados como privados están ocultos para otras clases, pero no están ocultos para otras instancias de la misma clase. En Java, el modificador private restringe el acceso al ámbito de la propia clase, no al objeto individual. Esto significa que cualquier método definido dentro de la clase puede acceder a los atributos privados de cualquier instancia de esa misma clase.

Cuando se define un método como calcularDistanciaAPunto(Punto otro), dicho método pertenece a la clase Punto. Por tanto, aunque los atributos sean privados, el método puede acceder tanto a los atributos del objeto actual como a los del objeto recibido como parámetro. La restricción de visibilidad no se aplica entre objetos de la misma clase, sino únicamente desde código externo a la clase.

public class Punto {
    private double x;
    private double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double calcularDistanciaAPunto(Punto otro) {
        double dx = this.x - otro.x;
        double dy = this.y - otro.y;
        return Math.sqrt(dx * dx + dy * dy);
    }
}


## 9. ¿Qué son los métodos "getter" y "setter" en los lenguajes orientados a objetos?

Los métodos getter y setter son métodos especiales que se utilizan para acceder y modificar atributos de una clase cuando estos se encuentran ocultos mediante encapsulación. Normalmente los atributos se declaran como privados para evitar accesos directos desde el exterior, y los getter y setter actúan como intermediarios controlados. De esta forma, se mantiene la protección de los datos internos sin impedir su uso cuando sea necesario.

Un método getter permite consultar el valor de un atributo. Su función es devolver el contenido de una variable privada sin exponerla directamente. Por su parte, un método setter permite modificar el valor de un atributo, pero de manera controlada. Dentro del setter pueden incluirse validaciones o comprobaciones que aseguren que el nuevo valor respeta las invariantes de la clase.

El uso de getter y setter refuerza la encapsulación porque evita el acceso directo a los atributos. Además, permite cambiar la implementación interna sin afectar al código externo, siempre que la interfaz pública se mantenga estable. Aunque en algunos casos puedan parecer simples métodos de acceso, su utilidad reside en el control que proporcionan sobre el estado interno de los objetos.

## 10. Cuando nos referimos a que la ocultación de información mejora la "seguridad" del programa, ¿nos referimos a que no pueda ser "hackeado"?

Cuando se afirma que la ocultación de información mejora la seguridad del programa, no se está haciendo referencia principalmente a la protección frente a ataques externos o a que el sistema no pueda ser hackeado. El término seguridad, en este contexto, se utiliza en un sentido más relacionado con la robustez y la fiabilidad del diseño interno del software.

La ocultación protege el estado interno de los objetos frente a usos indebidos por parte de otras partes del propio programa. Al impedir el acceso directo a los atributos y obligar a utilizar métodos controlados, se evita que se introduzcan valores inválidos o que se rompan las invariantes de la clase. Esto reduce errores lógicos y comportamientos inesperados, especialmente en proyectos grandes donde intervienen varios desarrolladores.

Por tanto, la mejora de la seguridad se refiere a una mayor protección frente a errores de programación y a un mejor control del flujo de datos dentro del sistema. Aunque un diseño bien encapsulado puede contribuir indirectamente a reducir ciertas vulnerabilidades, la ocultación de información no sustituye mecanismos específicos de seguridad informática como cifrado, autenticación o control de accesos externos.

## 11. ¿Qué diferencia hay entre **miembro de instancia** y **miembro de clase**? ¿Los miembros de clase también se pueden ocultar?

Un miembro de instancia es aquel que pertenece a cada objeto creado a partir de una clase. Cada vez que se crea una nueva instancia, se reserva memoria independiente para sus atributos de instancia. Por ejemplo, si una clase tiene un atributo no estático, cada objeto tendrá su propia copia de ese atributo. Estos miembros representan el estado particular de cada objeto y normalmente se accede a ellos a través de una referencia concreta.

Un miembro de clase, en cambio, es compartido por todas las instancias y pertenece a la propia clase, no a un objeto específico. En Java se declara con la palabra clave static. Solo existe una única copia en memoria, independientemente del número de objetos creados. Este tipo de miembro se utiliza cuando la información o el comportamiento es común a todos los objetos y no depende del estado individual de cada uno.

Los miembros de clase también pueden ocultarse mediante modificadores de visibilidad como private o public. El hecho de que un miembro sea estático no implica que deba ser público. Se puede declarar un atributo estático como private para impedir el acceso directo desde otras clases, obligando a utilizar métodos públicos controlados si se desea consultarlo o modificarlo. De este modo, la encapsulación se aplica tanto a miembros de instancia como a miembros de clase.

## 12. Brevemente: ¿Tiene sentido que los constructores sean privados?

Sí, puede tener sentido que los constructores sean privados en determinados diseños. Un constructor privado impide que se creen objetos desde fuera de la propia clase. Es decir, ninguna otra clase puede utilizar la palabra clave new para instanciar directamente ese tipo. Esto se utiliza cuando se desea controlar estrictamente cómo y cuándo se crean los objetos.

Una situación habitual es el patrón Singleton, donde se pretende que solo exista una única instancia de la clase durante toda la ejecución del programa. Al declarar el constructor como privado, se evita que el código externo cree múltiples objetos y se obliga a utilizar un método estático público que gestione la instancia única. También puede emplearse en clases que solo contienen métodos estáticos y no necesitan instanciarse.

Por tanto, aunque en muchos casos los constructores sean públicos, declararlos como privados es una herramienta de diseño que permite reforzar el control sobre la creación de objetos y garantizar determinadas restricciones en el uso de la clase.

## 13. ¿Cómo se indican los **miembros de clase** en Java? Pon un ejemplo, en la clase `Punto` definida anteriormente, para que incluya miembros de clase que permitan saber cuáles son los valores `x` e `y` máximos que se han establecido en todos los puntos que se hayan creado hasta el momento.

En Java, los miembros de clase se indican mediante la palabra clave static. Esto significa que pertenecen a la clase en sí misma y no a cada objeto individual. Solo existe una única copia compartida por todas las instancias. Se accede a ellos normalmente usando el nombre de la clase, no a través de un objeto concreto, lo que refleja que no dependen del estado particular de una instancia.

A continuación se muestra una versión de la clase Punto que incorpora miembros de clase para almacenar los valores máximos de x e y registrados entre todos los objetos creados hasta el momento. Estos atributos son estáticos y se actualizan cada vez que se construye un nuevo punto.

public class Punto {
    private double x;
    private double y;

    private static double maxX = Double.NEGATIVE_INFINITY;
    private static double maxY = Double.NEGATIVE_INFINITY;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;

        if (x > maxX) {
            maxX = x;
        }
        if (y > maxY) {
            maxY = y;
        }
    }

    public static double getMaxX() {
        return maxX;
    }

    public static double getMaxY() {
        return maxY;
    }
}

En este diseño, maxX y maxY son miembros de clase porque están declarados como static. Se comparten entre todos los objetos Punto y reflejan información global relativa a todas las instancias creadas. Los métodos estáticos getMaxX y getMaxY permiten acceder a esos valores sin necesidad de disponer de un objeto concreto.

## 14. Como sería un método factoría dentro de la clase `Punto` para construir un `Punto` a partir de dos coordenadas, pero que las redondee al entero más cercano. Escribe sólo el código del método, no toda la clase ¿Has usado `static`? 

public static Punto crearPuntoRedondeado(double x, double y) {
    int rx = (int) Math.round(x);
    int ry = (int) Math.round(y);
    return new Punto(rx, ry);
}

Sí, se ha utilizado static. Un método factoría debe ser de clase porque se invoca sin necesidad de disponer previamente de una instancia. Su función es precisamente encargarse de la creación del objeto, por lo que no puede depender de un objeto ya existente.

## 15. Cambia la implementación de `Punto`. En vez de dos `double`, emplea un array interno de dos posiciones, intentando no modificar la interfaz pública de la clase.

A continuación se muestra una posible implementación interna alternativa de la clase Punto utilizando un array de dos posiciones para almacenar las coordenadas. Se mantiene la misma interfaz pública (constructor, métodos de consulta y cálculo), de modo que el código externo no necesita modificarse.

public class Punto {
    private double[] coordenadas = new double[2];

    private static double maxX = Double.NEGATIVE_INFINITY;
    private static double maxY = Double.NEGATIVE_INFINITY;

    public Punto(double x, double y) {
        coordenadas[0] = x;
        coordenadas[1] = y;

        if (x > maxX) {
            maxX = x;
        }
        if (y > maxY) {
            maxY = y;
        }
    }

    public double calcularDistanciaAOrigen() {
        return Math.sqrt(coordenadas[0] * coordenadas[0] +
                         coordenadas[1] * coordenadas[1]);
    }

    public double getX() {
        return coordenadas[0];
    }

    public double getY() {
        return coordenadas[1];
    }

    public static double getMaxX() {
        return maxX;
    }

    public static double getMaxY() {
        return maxY;
    }
}

## 16. Si un atributo va a tener un método "getter" y "setter" públicos, ¿no es mejor declararlo público? ¿Cuál es la convención más habitual sobre los atributos, que sean públicos o privados? ¿Tiene esto algo que ver con las "invariantes de clase"?

Aunque un atributo disponga de métodos getter y setter públicos, no es recomendable declararlo público directamente. Si se hiciera público, cualquier parte del programa podría modificarlo sin ningún tipo de control. En cambio, mediante getter y setter se mantiene la posibilidad de introducir validaciones, restricciones o lógica adicional en el acceso, incluso aunque inicialmente solo se limite a devolver o asignar el valor.

La convención más habitual en programación orientada a objetos es declarar los atributos como privados. Esto refuerza la encapsulación y permite que la clase controle completamente su estado interno. Los métodos públicos forman parte de la interfaz visible, mientras que los datos internos quedan protegidos. Incluso cuando un setter simplemente asigna un valor, mantener el atributo privado deja abierta la posibilidad de cambiar la lógica en el futuro sin alterar la interfaz pública.

Esto está directamente relacionado con las invariantes de clase. Si los atributos fueran públicos, cualquier código externo podría modificarlos y romper las condiciones que garantizan que el objeto esté en un estado válido. Al mantenerlos privados y permitir su modificación solo a través de métodos controlados, resulta posible asegurar que cualquier cambio respete las reglas internas definidas por la clase.

## 17. ¿Qué significa que una clase sea **inmutable**? ¿qué es un método modificador? ¿Un método modificador es siempre un "setter"? ¿Tiene ventajas que una clase sea inmutable?

Una clase es inmutable cuando, una vez creado un objeto, su estado no puede modificarse. Es decir, los valores de sus atributos permanecen constantes durante toda la vida del objeto. Para lograrlo, los atributos suelen declararse privados y no se proporcionan métodos que permitan alterarlos después de la construcción. De esta forma, cualquier cambio requiere crear una nueva instancia en lugar de modificar la existente.

Un método modificador es aquel que altera el estado interno del objeto, es decir, cambia el valor de uno o varios atributos. No todos los métodos modificadores son necesariamente setters. Un setter es un tipo específico de método modificador cuya función principal es asignar un nuevo valor a un atributo concreto. Sin embargo, también pueden existir otros métodos que modifiquen el estado de manera más compleja, realizando cálculos o actualizaciones internas sin seguir el patrón típico de un setter.

Que una clase sea inmutable ofrece varias ventajas. Facilita el razonamiento sobre el programa, ya que el estado de los objetos no cambia inesperadamente. Reduce errores derivados de modificaciones accidentales y mejora la seguridad frente a problemas de concurrencia, ya que los objetos inmutables pueden compartirse entre distintos contextos sin riesgo de inconsistencias. Además, ayuda a mantener las invariantes de clase, porque el estado queda fijado en el momento de la construcción y no puede alterarse posteriormente.

## 18. ¿Es recomendable incluir métodos "setter" siempre y como convención?

No es recomendable incluir métodos setter de manera automática o como una convención obligatoria. La decisión de proporcionar un setter debe depender del diseño y de la responsabilidad de la clase. Si un atributo no debería cambiar después de la construcción del objeto, añadir un setter rompe esa intención y debilita la encapsulación.

Incluir setters sin necesidad puede facilitar modificaciones externas que no estaban previstas en el diseño inicial. Esto puede conducir a estados inconsistentes o a la ruptura de invariantes de clase. Además, una clase con demasiados setters tiende a convertirse en una simple estructura de datos expuesta, perdiendo parte de las ventajas de la orientación a objetos.

En muchos casos, es preferible proporcionar solo los métodos estrictamente necesarios para mantener el objeto en un estado válido. Si un cambio en el estado tiene sentido desde el punto de vista del modelo del problema, puede implementarse mediante un método que represente una operación significativa, en lugar de un setter genérico. De este modo, se mantiene un diseño más claro, controlado y coherente.

## 19. ¿La clase `String` en Java es mutable o inmutable? ¿Qué ocurre al concatenar dos cadenas? ¿Qué debemos hacer si vamos a hacer una operación que implique concatenar muchas veces para construir paso a paso una cadena muy larga?

La clase String en Java es inmutable. Una vez creado un objeto de tipo String, su contenido no puede modificarse. Cualquier operación que aparentemente altere la cadena en realidad produce un nuevo objeto con el resultado correspondiente. Esto significa que el valor original permanece intacto y no se modifica en memoria.

Al concatenar dos cadenas, por ejemplo mediante el operador +, no se altera ninguna de las cadenas originales. En su lugar, se crea un nuevo objeto String que contiene el resultado de la unión. Si se realizan muchas concatenaciones sucesivas, se generan múltiples objetos intermedios, lo que puede afectar al rendimiento y al consumo de memoria.

Cuando se necesita construir una cadena larga mediante muchas concatenaciones paso a paso, resulta más eficiente utilizar una clase mutable como StringBuilder. Esta permite modificar internamente el contenido sin crear un nuevo objeto en cada operación. Al finalizar el proceso, se puede obtener el String definitivo a partir del objeto StringBuilder, reduciendo así el coste en memoria y tiempo de ejecución.

## 20. En POO ¿Cómo se comparan objetos de una misma clase? ¿Por su contenido o por su identidad? ¿Qué es el método equals en Java? ¿Qué hace por defecto? ¿Cómo se deben comparar dos cadenas en Java? 

En programación orientada a objetos, los objetos pueden compararse de dos formas distintas: por identidad o por contenido. La comparación por identidad determina si dos referencias apuntan exactamente al mismo objeto en memoria. En cambio, la comparación por contenido analiza si los valores internos de los objetos son equivalentes según ciertos criterios definidos por la clase. Ambos enfoques tienen sentido dependiendo del contexto.

En Java, el operador == compara la identidad de los objetos cuando se utiliza con tipos de referencia. Es decir, verifica si ambas variables apuntan al mismo objeto. Para comparar el contenido se utiliza el método equals, que está definido en la clase base Object. Por defecto, equals se comporta igual que ==, es decir, compara identidad. Sin embargo, muchas clases redefinen este método para comparar el contenido de manera lógica.

En el caso de las cadenas, la clase String redefine el método equals para que compare el contenido carácter por carácter. Por ello, dos cadenas deben compararse utilizando equals y no el operador ==. Si se utilizara ==, solo se comprobaría si ambas referencias apuntan al mismo objeto, lo cual no garantiza que el texto almacenado sea igual.

## 21. ¿Qué son las clases "wrapper" en un lenguaje de programación orientado a objetos? ¿Cómo se hace? ¿Es un proceso automático? ¿Qué ventajas tienen? ¿Todos los lenguajes orientados a objetos tienen tipos primitivos y necesitan wrappers? 

Las clases wrapper son clases que encapsulan un valor primitivo dentro de un objeto. En Java existen wrappers para cada tipo primitivo, como Integer para int, Double para double o Boolean para boolean. Su finalidad es permitir que los valores primitivos puedan tratarse como objetos, lo cual es necesario en situaciones donde el lenguaje exige referencias a objetos, por ejemplo en colecciones genéricas.

En Java, la conversión entre un tipo primitivo y su clase wrapper puede realizarse de forma explícita o automática. Antiguamente era necesario crear manualmente el objeto wrapper a partir del valor primitivo y extraer después su valor. Sin embargo, desde versiones modernas del lenguaje, existe un mecanismo llamado autoboxing y unboxing que realiza esta conversión automáticamente cuando el contexto lo requiere.

Las ventajas principales de las clases wrapper son que permiten utilizar valores primitivos en estructuras que solo aceptan objetos, proporcionan métodos adicionales útiles y permiten representar la ausencia de valor mediante null, algo que no es posible con tipos primitivos. Además, integran los tipos básicos dentro del modelo orientado a objetos del lenguaje.

No todos los lenguajes orientados a objetos tienen tipos primitivos separados del resto. En algunos lenguajes, como Python, incluso los valores numéricos son objetos desde el principio, por lo que no se necesitan wrappers específicos. En cambio, en lenguajes como Java o C#, sí existe esta distinción y por ello se introducen clases wrapper para integrar ambos mundos.

## 22. ¿En POO qué es un **tipo de dato enumerado**? ¿En Java, un tipo de dato enumerado es una clase? ¿Qué ventajas tienen en términos de encapsulación los enumerados en Java?

Un tipo de dato enumerado es un tipo especial que permite definir un conjunto finito y fijo de valores posibles. En lugar de utilizar constantes numéricas o cadenas para representar estados o categorías, se define un tipo que solo puede tomar uno de los valores previamente declarados. Esto mejora la claridad del código y reduce errores, ya que se evita utilizar valores arbitrarios fuera del conjunto permitido.

En Java, un tipo enumerado definido con la palabra clave enum es, en realidad, una clase especial. Cada valor del enumerado es una instancia única de esa clase. Además, los enumerados pueden incluir atributos, constructores y métodos, lo que los convierte en una herramienta más potente que una simple lista de constantes.

En términos de encapsulación, los enumerados en Java ofrecen varias ventajas. Al restringir los valores posibles a un conjunto cerrado, se garantiza que no puedan crearse instancias adicionales desde fuera. El constructor del enum es implícitamente privado, lo que impide su instanciación manual. Además, al poder incluir métodos propios, el comportamiento asociado a cada valor puede definirse dentro del propio tipo, manteniendo juntos los datos y la lógica relacionada y reforzando así el diseño orientado a objetos.

## 23. Crea un tipo enumerado en Java que se llame `Mes`, con doce posibles instancias y que además proporcione métodos para obtener cuántos días tiene ese mes, el ordinal de ese mes en el año (1-12), empleando atributos privados y constructores del tipo enumerado. Añade además cuatro métodos para devolver si ese mes tiene algunos días de invierno, primavera, verano u otoño, indicando con un booleano el hemisferio (norte o sur, parámetro `enHemisferioNorte`). Es decir: `esDePrimavera(boolean esHemisferioNorte)`, `esDeVerano(boolean esHemisferioNorte)`, `esDeOtoño(boolean esHemisferioNorte)`, `esDeInvierno(boolean esHemisferioNorte)`

public enum Mes {

    ENERO(31, 1),
    FEBRERO(28, 2),
    MARZO(31, 3),
    ABRIL(30, 4),
    MAYO(31, 5),
    JUNIO(30, 6),
    JULIO(31, 7),
    AGOSTO(31, 8),
    SEPTIEMBRE(30, 9),
    OCTUBRE(31, 10),
    NOVIEMBRE(30, 11),
    DICIEMBRE(31, 12);

    private final int dias;
    private final int ordinal;

    private Mes(int dias, int ordinal) {
        this.dias = dias;
        this.ordinal = ordinal;
    }

    public int getDias() {
        return dias;
    }

    public int getOrdinal() {
        return ordinal;
    }

    public boolean esDePrimavera(boolean esHemisferioNorte) {
        if (esHemisferioNorte) {
            return this == MARZO || this == ABRIL || this == MAYO;
        } else {
            return this == SEPTIEMBRE || this == OCTUBRE || this == NOVIEMBRE;
        }
    }

    public boolean esDeVerano(boolean esHemisferioNorte) {
        if (esHemisferioNorte) {
            return this == JUNIO || this == JULIO || this == AGOSTO;
        } else {
            return this == DICIEMBRE || this == ENERO || this == FEBRERO;
        }
    }

    public boolean esDeOtoño(boolean esHemisferioNorte) {
        if (esHemisferioNorte) {
            return this == SEPTIEMBRE || this == OCTUBRE || this == NOVIEMBRE;
        } else {
            return this == MARZO || this == ABRIL || this == MAYO;
        }
    }

    public boolean esDeInvierno(boolean esHemisferioNorte) {
        if (esHemisferioNorte) {
            return this == DICIEMBRE || this == ENERO || this == FEBRERO;
        } else {
            return this == JUNIO || this == JULIO || this == AGOSTO;
        }
    }
}
