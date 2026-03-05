<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Excepciones". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 3. Excepciones

## 1. Empecemos un tema sobre control de errores en lenguajes de programación, con algo básico. En C, donde no existen las excepciones, pongamos un ejemplo de una raíz que toma número flotante positivo. Queremos controlar el error si la función recibe un número negativo. El usuario debe ser informado pero desde fuera de la función `raiz` ¿Cómo indicamos ese error?. Enumera dos opciones diferentes de diseñar, poniendo un ejemplo de código de cada una.

En C, al no existir un mecanismo de excepciones como en otros lenguajes, el control de errores debe diseñarse explícitamente mediante valores de retorno o mediante el uso de parámetros adicionales. La función no suele informar directamente al usuario del error, sino que devuelve algún tipo de indicación que permita al código que la invoca detectar el problema y actuar en consecuencia. De este modo se separa el cálculo de la raíz de la gestión del error, permitiendo que sea el programa principal quien decida cómo informar al usuario.

Una primera opción consiste en devolver un valor especial que indique que se ha producido un error. En el caso de una raíz cuadrada, si la función recibe un número negativo puede devolver un valor que no sea válido para el cálculo esperado. El código que llama a la función comprobaría ese valor antes de utilizar el resultado.

#include <stdio.h>
#include <math.h>

float raiz(float x) {
    if (x < 0) {
        return -1.0;  // valor especial para indicar error
    }
    return sqrt(x);
}

int main() {
    float num = -4;
    float r = raiz(num);

    if (r == -1.0) {
        printf("Error: no se puede calcular la raiz de un numero negativo\n");
    } else {
        printf("Resultado: %f\n", r);
    }
}

Una segunda opción consiste en devolver un código de estado e informar del resultado mediante un parámetro adicional (normalmente un puntero). En este diseño la función indica si la operación se ha realizado correctamente mediante el valor de retorno, mientras que el resultado del cálculo se escribe en una variable proporcionada por quien llama a la función. Este enfoque permite diferenciar claramente entre el resultado de la operación y el estado de la ejecución.

#include <stdio.h>
#include <math.h>

int raiz(float x, float *resultado) {
    if (x < 0) {
        return 0;  // código de error
    }
    *resultado = sqrt(x);
    return 1;  // ejecución correcta
}

int main() {
    float num = -4;
    float r;

    if (!raiz(num, &r)) {
        printf("Error: no se puede calcular la raiz de un numero negativo\n");
    } else {
        printf("Resultado: %f\n", r);
    }
}

## 2. Brevemente ¿Qué es una **"excepción"**? ¿Con qué objetivo las usa un programador cuando implementa funciones o cuando las llama?

Una excepción es un mecanismo de control de errores que utilizan algunos lenguajes de programación para indicar que durante la ejecución de un programa se ha producido una situación anómala o inesperada. En lugar de devolver códigos de error o valores especiales, el propio sistema de ejecución genera un objeto que representa el problema y que interrumpe el flujo normal del programa hasta que dicho problema es tratado. Este mecanismo permite separar claramente el código que realiza la operación principal del código encargado de gestionar los errores.

Las excepciones se utilizan con el objetivo de detectar y comunicar errores de forma estructurada entre distintas partes del programa. Cuando una función encuentra una situación que le impide continuar correctamente (por ejemplo, un dato inválido o un recurso que no puede utilizarse), puede lanzar una excepción. Esa excepción se propaga hacia las funciones que han realizado la llamada hasta que alguna de ellas decide capturarla y gestionar el problema.

Desde el punto de vista del programador que implementa funciones, las excepciones permiten indicar condiciones de error sin tener que modificar el valor de retorno de la función ni añadir parámetros adicionales para comunicar fallos. Desde el punto de vista del programador que llama a esas funciones, las excepciones permiten separar el código normal del código de tratamiento de errores, haciendo que el programa resulte más claro y más fácil de mantener.

## 3. Reescribe el mismo ejemplo de raiz, pero en Java, metiendo ese método en una clase `Calculadora` y llama a dicho método desde el método `main`, mostrando cómo se puede controlar desde fuera.

En Java, el control de errores puede realizarse mediante el uso de excepciones. Cuando una función detecta una situación que impide continuar con el cálculo, puede lanzar una excepción en lugar de devolver un valor especial o un código de error. De esta forma, la función que realiza el cálculo se limita a indicar que ha ocurrido un problema, mientras que el código que la llama es el encargado de decidir cómo tratar ese error. Esto permite separar claramente el cálculo de la gestión del fallo.

En el ejemplo de la raíz cuadrada, el método puede comprobar si el número recibido es negativo. Si lo es, se lanza una excepción indicando que el valor no es válido. El método no informa directamente al usuario, sino que simplemente comunica el problema. Posteriormente, el método main puede capturar esa excepción y decidir cómo reaccionar, por ejemplo mostrando un mensaje de error.

class Calculadora {

    public static double raiz(double x) {
        if (x < 0) {
            throw new IllegalArgumentException("No se puede calcular la raiz de un numero negativo");
        }
        return Math.sqrt(x);
    }
}

public class Main {

    public static void main(String[] args) {
        double numero = -4;

        try {
            double resultado = Calculadora.raiz(numero);
            System.out.println("Resultado: " + resultado);
        } catch (IllegalArgumentException e) {
            System.out.println("Error: " + e.getMessage());
        }
    }
}

En este diseño, el método raiz se encarga únicamente de realizar la operación y de lanzar la excepción cuando detecta un valor inválido. El control del error se realiza desde fuera del método, en el bloque try-catch del main, donde se captura la excepción y se informa al usuario. De esta manera se mantiene separada la lógica del cálculo y la lógica de tratamiento de errores.

## 4. ¿Qué es **"lanzar"** una excepción? ¿Qué es **"controlar"** o **"capturar"** una excepción? ¿Qué es que se **"propague"** una excepción? ¿Qué le va ocurriendo a las funciones en la pila de llamadas por donde se va propagando la excepción? ¿Las funciones que no la controlan se reanudan después de alguna forma? Explica con el mismo ejemplo anterior en Java de la raíz cuadrada.

Lanzar una excepción significa que, durante la ejecución de un método, se detecta una situación anómala y se genera una excepción para indicar que no es posible continuar con la ejecución normal. En Java esto se hace mediante la instrucción throw. Cuando un método lanza una excepción, la ejecución normal del método se interrumpe inmediatamente y se inicia el proceso de búsqueda de un lugar en el programa donde esa excepción pueda ser tratada.

Capturar o controlar una excepción significa interceptarla mediante un bloque try-catch para poder reaccionar ante el error. Cuando una excepción es capturada, el programa puede decidir qué hacer: mostrar un mensaje, intentar una solución alternativa o finalizar la operación de forma controlada. Si un método lanza una excepción y el método que lo llamó no la captura, entonces la excepción se propaga hacia el método que está más arriba en la pila de llamadas.

En el ejemplo de la raíz cuadrada, el método raiz de la clase Calculadora lanza una excepción si recibe un número negativo. El método main es quien realiza la llamada y quien decide capturar esa excepción. De esta forma el método que realiza el cálculo no se encarga de informar al usuario, sino que simplemente comunica que ha ocurrido un error.

class Calculadora {

    public static double raiz(double x) {
        if (x < 0) {
            throw new IllegalArgumentException("Numero negativo");
        }
        return Math.sqrt(x);
    }
}

public class Main {

    public static void main(String[] args) {
        double numero = -4;

        try {
            double resultado = Calculadora.raiz(numero);
            System.out.println("Resultado: " + resultado);
        } catch (IllegalArgumentException e) {
            System.out.println("Error: " + e.getMessage());
        }
    }
}

Cuando el método raiz lanza la excepción, su ejecución termina inmediatamente y el método abandona la pila de llamadas. El sistema de ejecución busca entonces un bloque catch adecuado en los métodos que lo habían llamado. Si el método que hizo la llamada no captura la excepción, esta continúa propagándose hacia arriba en la pila de llamadas. Cada método por el que pasa la excepción se termina sin completar su ejecución normal. Las funciones que no capturan la excepción no se reanudan después, sino que se finalizan mientras la excepción sigue propagándose hasta encontrar un controlador o hasta que el programa termina.

## 5. ¿Qué ventajas tiene frente a C, la **"propagación natural"** de las excepciones a través de la pila (*stack*) de llamadas?

La propagación natural de las excepciones permite que un error detectado en una función se transmita automáticamente hacia las funciones que la han llamado, siguiendo la pila de llamadas. Si un método no captura la excepción, el sistema de ejecución la envía al método que está inmediatamente por encima en la pila, y así sucesivamente hasta que se encuentre un bloque que la capture o hasta que termine el programa. Este mecanismo evita que cada función tenga que comprobar manualmente si se ha producido un error después de cada llamada.

Una ventaja importante frente a C es que no es necesario propagar manualmente los errores mediante códigos de retorno. En C, cada función que llama a otra debe comprobar su valor de retorno y decidir si debe devolver también un error al nivel superior. Esto obliga a añadir comprobaciones repetitivas y puede hacer que el código principal quede mezclado con lógica de control de errores. Con excepciones, el flujo normal del programa se mantiene limpio y el tratamiento de errores puede concentrarse en lugares concretos.

Otra ventaja es que solo se controla el error en el nivel donde realmente tiene sentido hacerlo. Muchas funciones intermedias no tienen suficiente información para decidir cómo actuar ante un fallo, por lo que simplemente dejan que la excepción continúe propagándose. De esta forma, el error puede gestionarse en una capa superior del programa que tenga el contexto adecuado para decidir qué hacer, como informar al usuario, repetir la operación o finalizar la ejecución.

Finalmente, la propagación automática mejora la claridad y mantenibilidad del código. El código que implementa la funcionalidad principal no necesita estar lleno de comprobaciones de error después de cada llamada. Además, se reduce el riesgo de olvidar verificar un código de retorno, algo que en C puede provocar que un error pase desapercibido y genere comportamientos incorrectos más adelante en el programa.

## 6. En orientación a objetos, ¿las excepciones suelen ser objetos? ¿Qué ventajas tiene esto en términos de encapsulación? ¿Podemos entonces crear excepciones personalizadas?

En los lenguajes orientados a objetos, como Java, las excepciones se representan normalmente como objetos. Esto significa que una excepción pertenece a una clase y forma parte de una jerarquía de clases específica para el tratamiento de errores. Cuando se produce un problema durante la ejecución de un programa, lo que realmente se lanza es una instancia de una de esas clases. Este enfoque encaja naturalmente con el modelo de programación orientado a objetos, donde la información y el comportamiento se organizan en objetos.

El hecho de que las excepciones sean objetos aporta ventajas en términos de encapsulación. Una excepción puede contener información relevante sobre el error ocurrido, como un mensaje descriptivo o datos adicionales que ayuden a entender la causa del problema. Esa información queda encapsulada dentro del propio objeto excepción, de modo que el código que captura la excepción puede acceder a ella de forma controlada mediante métodos definidos en la clase correspondiente.

Además, al formar parte de una jerarquía de clases, las excepciones pueden aprovechar los mecanismos habituales de la orientación a objetos, como la herencia. Esto permite definir distintos tipos de errores relacionados entre sí y tratarlos de forma más específica o más general según convenga. Por ejemplo, un programa puede capturar una excepción concreta o una clase más general que represente un conjunto de errores similares.

Como consecuencia de este diseño, es posible crear excepciones personalizadas. Un programador puede definir nuevas clases de excepción que representen errores específicos de una aplicación. De esta manera, el programa puede comunicar situaciones de error de forma más clara y precisa, adaptándose mejor a las necesidades del problema que se está resolviendo.

## 7. En relación con las ventajas de la encapsulación, comparando el ejemplo en C con Java. ¿Qué **información esencial** lleva cualquier **objeto excepción** que es muy útil tener cuando se llega a un manejador?

En Java, al ser las excepciones objetos, cada una de ellas puede encapsular información sobre el error ocurrido. Cuando la excepción llega a un manejador (`catch`), ese objeto contiene datos que permiten entender mejor qué ha sucedido y en qué contexto ocurrió el problema. Esta información viaja junto con la excepción mientras se propaga por la pila de llamadas, por lo que está disponible cuando finalmente se captura.

Una de las informaciones más útiles que contiene cualquier objeto excepción es un mensaje descriptivo del error. Este mensaje se establece normalmente cuando se crea la excepción y sirve para indicar de forma clara cuál ha sido el problema detectado. Cuando el manejador captura la excepción, puede obtener ese mensaje y utilizarlo para mostrar información al usuario o para registrar el error en un sistema de diagnóstico.

Otra información esencial es la traza de la pila de llamadas (stack trace). Esta traza indica la secuencia de métodos que estaban ejecutándose en el momento en que se produjo el error, mostrando exactamente en qué método y en qué línea del programa se originó la excepción. Gracias a esta información resulta mucho más fácil localizar el origen del problema durante el desarrollo o el mantenimiento del programa.

Comparado con el ejemplo en C, donde normalmente solo se devuelve un valor especial o un código de error, las excepciones en Java proporcionan mucha más información contextual sobre el fallo. En C, si se quiere conocer más detalles sobre el error, esa información debe transmitirse manualmente mediante variables adicionales o estructuras de datos. En cambio, con las excepciones esa información ya está integrada dentro del propio objeto que representa el error.

## 8. En Java, sobre el bloque **"try-catch"**, ¿se pueden tener más de un bloque `catch`? ¿cuántos bloques `catch` se ejecutan?

En Java, un bloque `try` puede ir seguido de varios bloques `catch`. Cada uno de estos bloques está preparado para capturar un tipo concreto de excepción. Esto permite que un mismo fragmento de código pueda manejar distintos errores de forma diferente, dependiendo del tipo de excepción que se produzca durante la ejecución.

Cuando ocurre una excepción dentro del bloque `try`, el sistema busca entre los bloques `catch` el primero que sea compatible con el tipo de excepción lanzada. La comprobación se realiza siguiendo el orden en el que aparecen los `catch`. Si se encuentra uno adecuado, ese bloque se ejecuta y se considera que la excepción ha sido tratada.

Sin embargo, solo se ejecuta un bloque `catch` por cada excepción lanzada. Una vez que un `catch` captura la excepción, el resto de bloques `catch` asociados a ese `try` ya no se evalúan. Después de ejecutarse el bloque que maneja la excepción, el programa continúa su ejecución a partir de la parte que sigue al conjunto `try-catch`.

Por esta razón, cuando se utilizan varios bloques `catch`, es importante organizarlos correctamente. Normalmente se colocan primero los manejadores de excepciones más específicas y después los más generales, para evitar que una excepción específica sea capturada prematuramente por un manejador demasiado general.

## 9. Si las excepciones producen rupturas en el código llamador, ¿cómo podemos garantizar que se ejecuta siempre finalmente un código necesario para cierre de ficheros, liberacion de recursos, antes de que continúe propagándose la excepción? Pon un ejemplo en Java con `finally`, tanto con `catch` como sin él.

Cuando se produce una excepción, la ejecución normal del programa se interrumpe y comienza el proceso de propagación de la excepción hacia los métodos que se encuentran más arriba en la pila de llamadas. Sin embargo, en muchas situaciones es necesario garantizar que ciertas operaciones se ejecuten siempre antes de abandonar un bloque de código, como cerrar ficheros, liberar recursos o realizar tareas de limpieza. Para ello, Java proporciona el bloque finally.

El bloque finally se asocia a un bloque try y contiene código que se ejecuta siempre, independientemente de que se haya producido o no una excepción. Esto significa que se ejecuta tanto si el código del try termina correctamente como si se lanza una excepción y se captura con un catch. Incluso si la excepción continúa propagándose, el código del finally se ejecutará antes de que el control abandone ese bloque.

Un primer caso es el uso de finally junto con un bloque catch. En este diseño, el catch se encarga de manejar la excepción y el finally garantiza que el código de limpieza se ejecute siempre.

try {
    int resultado = Calculadora.raiz(-4);
    System.out.println("Resultado: " + resultado);
} catch (IllegalArgumentException e) {
    System.out.println("Error: " + e.getMessage());
} finally {
    System.out.println("Liberando recursos o cerrando ficheros");
}

También es posible utilizar finally sin incluir un bloque catch. En ese caso, si se produce una excepción, esta continuará propagándose hacia los métodos superiores, pero el código del finally se ejecutará igualmente antes de que eso ocurra.

try {
    int resultado = Calculadora.raiz(-4);
    System.out.println("Resultado: " + resultado);
} finally {
    System.out.println("Liberando recursos o cerrando ficheros");
}

## 10. En Java, el bloque `finally` puede ir sin `catch`? ¿Se ejecuta siempre tanto si ocurre como si no ocurre una excepción? ¿Y si hay un `return` en medio del `try`?

En Java, el bloque `finally` puede utilizarse sin un bloque `catch`, siempre que esté asociado a un bloque `try`. En este caso, el objetivo principal del `finally` es asegurar la ejecución de cierto código que debe realizarse independientemente de si ocurre o no una excepción. Si durante la ejecución del `try` se lanza una excepción y no hay un `catch` que la capture, esa excepción seguirá propagándose hacia los métodos superiores de la pila de llamadas, pero antes de que esto ocurra se ejecutará el contenido del `finally`.

El bloque `finally` se ejecuta tanto si ocurre una excepción como si no ocurre. Si el código del `try` termina normalmente, el programa ejecutará el bloque `finally` antes de continuar con el resto del programa. Si se produce una excepción y existe un `catch` que la captura, primero se ejecutará el `catch` y posteriormente el `finally`. Si no existe un `catch`, el `finally` se ejecutará igualmente antes de que la excepción continúe propagándose.

Incluso si dentro del bloque `try` aparece una instrucción `return`, el bloque `finally` también se ejecuta antes de que el método termine realmente. El valor que se va a devolver se calcula en el momento del `return`, pero la salida efectiva del método se retrasa hasta que el bloque `finally` ha terminado su ejecución. Esto permite asegurar que las tareas de limpieza o liberación de recursos se realicen siempre antes de abandonar el método.

## 11. En Java, qué son las excepciones **"controladas"** y las **"no controladas"**? ¿Qué papel juega `RuntimeException`? Pon un ejemplo de excepciones típicas controladas y no controladas que incluso nosotros mismos podríamos usar. Haz dos listas con 3 o 4 ejemplos de situación donde se suele preferir una excepción controlada y donde se suele preferir una excepción no controlada.

En Java existen dos grandes categorías de excepciones: excepciones controladas (checked) y excepciones no controladas (unchecked). Las excepciones controladas son aquellas que el compilador obliga a tratar explícitamente. Cuando un método puede producir una de estas excepciones, el programador debe capturarla con un `try-catch` o declararla en la firma del método para indicar que puede propagarse. En cambio, las excepciones no controladas no requieren tratamiento obligatorio por parte del compilador y pueden propagarse libremente sin necesidad de ser declaradas.

El papel de `RuntimeException` es fundamental en esta clasificación. Todas las excepciones que heredan de `RuntimeException` se consideran no controladas. Estas excepciones suelen representar errores de programación o situaciones que normalmente no deberían producirse si el programa está bien diseñado. Por ejemplo, acceder a una posición inválida de un array o utilizar una referencia nula. Debido a esto, el lenguaje no obliga a capturarlas, aunque el programador puede hacerlo si lo considera necesario.

Ejemplos de excepciones controladas típicas son aquellas relacionadas con operaciones externas o condiciones que pueden ocurrir en circunstancias normales y que el programa puede intentar manejar:

* Lectura o escritura de un fichero que puede no existir o no tener permisos.
* Acceso a un recurso de red que puede fallar por problemas de conexión.
* Procesamiento de datos de entrada que pueden tener un formato incorrecto.
* Operaciones con bases de datos que pueden fallar por problemas de acceso.

Ejemplos de excepciones no controladas típicas son situaciones que suelen indicar errores en la lógica del programa:

* Acceder a una posición de un array que está fuera de sus límites.
* Intentar utilizar una referencia que tiene valor nulo.
* Realizar una división entera entre cero.
* Pasar a un método un argumento que viola una precondición del propio método.

En general, se prefiere utilizar excepciones controladas cuando el error puede ocurrir de forma razonable durante la ejecución normal del programa y existe alguna forma de reaccionar ante él. En cambio, las excepciones no controladas suelen utilizarse cuando el problema indica un fallo de programación o una situación que normalmente no debería recuperarse en tiempo de ejecución, dejando que el error se propague para ser detectado y corregido durante el desarrollo.

## 12. ¿Qué es y para qué se usa `throws`? ¿Por qué es alternativa a capturar una excepción controlada?

En Java, la palabra clave `throws` se utiliza en la declaración de un método para indicar que ese método puede lanzar una o varias excepciones. Cuando un método incluye `throws` en su firma, está informando a quien lo utilice de que durante su ejecución podría producirse una determinada excepción y que esta no será tratada dentro del propio método. De esta manera, el método delega la responsabilidad de gestionar ese posible error en el código que lo llame.

El uso de `throws` es especialmente relevante en el caso de excepciones controladas (checked), ya que el compilador obliga a que estas sean tratadas de alguna forma. Existen dos opciones: capturar la excepción dentro del propio método mediante un bloque `try-catch`, o bien declararla con `throws` para que pueda propagarse hacia los métodos superiores de la pila de llamadas. Si se utiliza `throws`, el compilador exigirá que el método que realiza la llamada también capture la excepción o la vuelva a declarar.

Por esta razón, `throws` se considera una alternativa a capturar una excepción controlada dentro del método. En lugar de manejar el error en ese mismo nivel, el método permite que la excepción continúe propagándose hasta llegar a una parte del programa donde tenga más sentido tratarla. Esto resulta útil cuando el método que detecta el problema no tiene suficiente contexto para decidir cómo actuar ante el error.

En términos de diseño del programa, `throws` ayuda a mantener una separación clara entre la lógica que realiza una operación y la lógica encargada de manejar posibles fallos. Además, al quedar declaradas las excepciones en la firma del método, el propio código documenta qué tipos de problemas pueden producirse durante su ejecución, facilitando así su uso y mantenimiento.

## 13. Pon un ejemplo en Java de firma de método que incluya `throws`, de una función que abre un fichero pero que declara que no le interesa menejar la excepción de si el fichero no existe, sino que se propague hacia arriba. Eso sí, acuérdate del `finally`.

En Java, un método puede declarar en su firma que puede lanzar una excepción mediante la palabra clave throws. Esto indica que el método no va a encargarse de gestionar ese error internamente, sino que permitirá que la excepción se propague hacia el método que lo haya llamado. En el caso de operaciones con ficheros, es habitual que se pueda producir una excepción si el fichero no existe o no puede abrirse, por lo que el método puede declarar esa posibilidad en su firma.

En este ejemplo se muestra un método que intenta abrir un fichero para leerlo. El método declara que puede lanzar la excepción correspondiente si el fichero no existe, utilizando throws. De esta forma, no se incluye ningún bloque catch dentro del método. Sin embargo, se utiliza un bloque finally para garantizar que el recurso asociado al fichero se cierre correctamente si ha llegado a abrirse.

import java.io.*;

class Utilidades {

    public static void leerFichero(String nombre) throws FileNotFoundException, IOException {
        BufferedReader lector = null;

        try {
            lector = new BufferedReader(new FileReader(nombre));
            String linea = lector.readLine();
            System.out.println(linea);
        } finally {
            if (lector != null) {
                try {
                    lector.close();
                } catch (IOException e) {
                    System.out.println("Error al cerrar el fichero");
                }
            }
        }
    }
}

En este diseño, el método leerFichero no captura la excepción que puede producirse si el fichero no existe. En su lugar, declara esa posibilidad mediante throws, lo que permite que la excepción se propague hacia el método que realice la llamada. No obstante, el bloque finally garantiza que el recurso asociado al fichero se cierre correctamente antes de que el método termine o antes de que la excepción continúe propagándose por la pila de llamadas.

## 14. ¿Podemos poner en `throws` excepciones no controladas, como `RuntimeException`? ¿Debería el método llamador entonces poner `try-catch` en ese caso? ¿Qué sentido tendría?

En Java es posible incluir excepciones no controladas (las que heredan de `RuntimeException`) en la cláusula `throws` de la firma de un método. El lenguaje no lo prohíbe, aunque tampoco es necesario hacerlo. A diferencia de las excepciones controladas, el compilador no obliga a declararlas ni a capturarlas, por lo que un método puede lanzar una excepción de este tipo sin que aparezca en su firma.

Si un método declara en `throws` una excepción no controlada, el método llamador no está obligado a utilizar un bloque `try-catch` para tratarla. El compilador permitirá llamar a ese método sin ningún tratamiento explícito de la excepción. Sin embargo, el programador podría decidir capturarla igualmente si desea reaccionar ante esa situación concreta, por ejemplo registrando el error o transformándolo en otro tipo de excepción.

Declarar excepciones no controladas en `throws` puede tener un valor informativo o documental. Al aparecer en la firma del método, se está indicando a quien utilice ese método qué tipos de errores podrían producirse durante su ejecución. Esto puede ayudar a comprender mejor las precondiciones del método o los posibles problemas que pueden surgir al utilizarlo.

En términos prácticos, el sentido de declarar una excepción no controlada en `throws` es hacer explícito el contrato del método, aunque el lenguaje no obligue a hacerlo. El método llamador puede decidir ignorarla y dejar que se propague, o bien capturarla si considera que tiene sentido manejar ese tipo de error en ese punto del programa.

## 15. ¿Cuándo se recomienda usar excepciones controladas, como `IOException`, y cuándo no controladas como `IllegalArgumentException`? ¿Existen en todos los lenguajes ambas opciones? En los que sólo existe una opción, ¿cuál es la más habitual?

Se recomienda usar excepciones controladas cuando el fallo es plausible en condiciones normales de ejecución y, además, el código que llama puede razonablemente recuperarse o tomar una decisión alternativa. Son típicas en operaciones dependientes del entorno: entrada/salida, ficheros, red, bases de datos o recursos externos. En esos casos, obligar a tratar o declarar la excepción hace explícito el “contrato” del método y fuerza a que el llamador decida si gestiona el problema o lo deja propagarse.

Se recomienda usar excepciones no controladas cuando el problema suele indicar un error de programación, una violación de precondiciones o un estado interno incoherente. Es el caso de argumentos inválidos (por ejemplo, un parámetro negativo donde no tiene sentido), uso incorrecto de una API, índices fuera de rango, referencias nulas o invariantes rotas. En estas situaciones, obligar a capturar en cada llamada suele generar código repetitivo y poco útil, y lo habitual es corregir el origen del error en vez de “recuperarse” en tiempo de ejecución.

No todos los lenguajes ofrecen ambas opciones como en Java. Hay lenguajes donde prácticamente todas las excepciones se comportan como no controladas (no existe obligación del compilador de declararlas o capturarlas), y otros lenguajes donde el control de errores se hace principalmente con códigos de retorno, valores especiales o tipos de resultado en lugar de excepciones. Cuando solo existe una modalidad de excepciones, la más habitual en la práctica es el modelo no controlado, donde las excepciones pueden lanzarse y capturarse, pero no requieren declaración obligatoria en la firma del método.

Incluso en lenguajes con un único modelo de excepciones, suele mantenerse la misma intención de diseño: reservar los errores de uso (precondiciones, errores de programación) para excepciones y manejar fallos esperables del entorno con mecanismos explícitos o con excepciones que el llamador decide capturar en un nivel adecuado. La diferencia es que, sin excepciones controladas, esa disciplina no la impone el compilador, sino que se establece por convención, documentación y buenas prácticas del proyecto.

## 16. ¿Tiene sentido lanzar excepciones dentro del `catch`? ¿Se puede relanzar la misma excepción capturada? ¿Cuándo tendría sentido hacer esto último? Pon ejemplos de ambos casos.

Tiene sentido lanzar excepciones dentro de un catch cuando, tras detectar y capturar un error, se necesita comunicar el fallo de otra forma al nivel superior. Esto se usa para “traducir” una excepción de bajo nivel (por ejemplo, de acceso a fichero) en una excepción más significativa para la capa actual (por ejemplo, un error de negocio), o para envolverla añadiendo contexto. En este caso, el catch no “resuelve” el problema, sino que lo convierte en una señal más útil para quien llama.

También se puede relanzar la misma excepción que se ha capturado. En Java se hace volviendo a ejecutar throw con la misma referencia capturada. Esto tiene sentido cuando el catch solo realiza una acción auxiliar (registrar el error, liberar recursos adicionales, actualizar un estado, añadir trazas) pero no debe decidir la recuperación. De esta forma se garantiza que el error no se silencie y que el nivel superior siga teniendo la oportunidad de gestionarlo.

Ejemplo de lanzar una excepción nueva desde el catch (envolver o traducir):

import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;

class ServicioLectura {

    static String primeraLinea(String ruta) {
        try (BufferedReader br = new BufferedReader(new FileReader(ruta))) {
            return br.readLine();
        } catch (IOException e) {
            // Se añade contexto y se cambia el tipo a una excepción más adecuada para esta capa
            throw new IllegalStateException("Fallo al leer el fichero: " + ruta, e);
        }
    }
}

Ejemplo de relanzar la misma excepción capturada (capturar para una acción auxiliar y repropagar):

import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;

class ServicioLectura {

    static String primeraLinea(String ruta) throws IOException {
        try (BufferedReader br = new BufferedReader(new FileReader(ruta))) {
            return br.readLine();
        } catch (IOException e) {
            // Acción auxiliar (por ejemplo, registrar); después se repropaga exactamente la misma excepción
            System.err.println("Error de E/S al acceder a: " + ruta);
            throw e;
        }
    }
}

## 17. ¿En qué consiste que una excepción sea la **"causa"** de otra excepción? Pon un ejemplo en Java, donde capturemos una excepción de bajo nivel y la encapsulemos en otra personalizada de alto nivel. Cuando una excepción sale por pantalla y tiene una causa, ¿se ve?

Que una excepción sea la “causa” de otra significa que una excepción de más alto nivel se crea para representar un problema en una capa del programa, pero conserva internamente la excepción original que provocó ese fallo. De esta forma, la nueva excepción encapsula a la anterior y mantiene la relación entre ambas. Este mecanismo permite añadir contexto más adecuado para la capa actual del programa sin perder la información técnica del error original.

Este enfoque se utiliza con frecuencia cuando se captura una excepción de bajo nivel (por ejemplo, relacionada con acceso a ficheros o red) y se convierte en una excepción más representativa para la lógica de la aplicación. Así, la capa superior del programa no necesita conocer detalles internos de implementación, pero sigue existiendo un enlace con la excepción original para depuración o diagnóstico.

import java.io.*;

class ErrorDatosException extends Exception {
    public ErrorDatosException(String mensaje, Throwable causa) {
        super(mensaje, causa);
    }
}

class ServicioDatos {

    public static String leerPrimeraLinea(String ruta) throws ErrorDatosException {
        try {
            BufferedReader lector = new BufferedReader(new FileReader(ruta));
            String linea = lector.readLine();
            lector.close();
            return linea;
        } catch (IOException e) {
            throw new ErrorDatosException("Error al acceder a los datos del sistema", e);
        }
    }
}

En este ejemplo se captura una excepción de bajo nivel (IOException) y se encapsula dentro de una excepción personalizada (ErrorDatosException). Cuando una excepción que tiene una causa se muestra por pantalla, por ejemplo mediante un método que imprime la traza de la pila, normalmente se visualiza también la cadena de excepciones. Esto permite ver primero la excepción principal y, a continuación, la causa que la originó, lo que facilita identificar el origen real del problema.
