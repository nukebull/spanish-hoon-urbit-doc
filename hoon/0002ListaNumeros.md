## Lista de números
Este código de ejemplo está destinado a familiarizarlo con los conceptos básicos de la sintaxis de Hoon. Está bien si no entiendes todo de inmediato; algunos conceptos pueden estar más allá de su alcance por ahora. Lo importante es que te acostumbres a los elementos del código y su aspecto una vez combinados en un programa válido.

A continuación se muestra un programa simple de Hoon que toma un solo número n del usuario como entrada y produce una lista de números de 1 hasta n-1. Por lo tanto, si el usuario da el número 5, el programa va a producir: ~[1 2 3 4].
~~~
|=  end=@                                               ::  1
=/  count=@  1                                          ::  2
|-                                                      ::  3
^-  (list @)                                            ::  4
?:  =(end count)                                        ::  5
  ~                                                     ::  6
:-  count                                               ::  7
$(count (add 1 count))                                  ::  8
~~~
Como mencionamos en la lección anterior, la forma más fácil de usar dicho programa es ejecutarlo como generador. Monta tu escritorio **|mount %** (si no lo hiciste antes), guardar un archivo en /home/gen,dentro del directorio de tu nave y confirmar estos cambios **|commit %home** Esto te permite ejecutarlo desde el Dojo (línea de comando) de tu nave como generador. Guarde el código anterior allí como list.hoon. Ahora puedes ejecutarlo en el Dojo así:
~~~
~your-ship:> +list 5
~~~

¡Intentalo! Puede elegir cualquier número natural para poner después +list, pero no puede ser cero o en blanco. El **+** le permite al Dojo saber que está buscando un generador con el siguiente nombre. No se escribe la parte **.hoon** de la extensión del archivo cuando lo ejecuta en el Dojo.

Pero, ¿qué todos estos símbolos onduladas en el programa hacen ? Probablemente no esté claro de inmediato. Repasemos cada componente de este código.

### El código
Probablemente notó que hay una columna de dos puntos y números en nuestro ejemplo. El símbolo **::**  le dice al compilador que ignore el resto del texto en la línea. Dicho texto se conoce como un "comentario", en lugar de realizar un cálculo, explica las cosas a los lectores humanos del código fuente.

En nuestro programa de ejemplo, usamos comentarios con números de línea para una referencia conveniente, de modo que podamos profundizar en el código línea por línea. Los conceptos importantes de Hoon estarán en negrita cuando se mencionen por primera vez.

#### Línea 1
~~~
|=  end=@
~~~
Están sucediendo algunas cosas en nuestra primera línea. La primera parte **|=**, es una especie de runa (**rune**) . Las runas son los componentes básicos de todo el código Hoon, representado como un par de caracteres ASCII no alfanuméricos. Las runas forman expresiones; Las runas se usan como las palabras clave se usan en otros idiomas. En otras palabras, todos los cálculos en Hoon finalmente requieren runas. Las runas y otras expresiones de Hoon están separadas entre sí por dos espacios o un salto de línea.

Todas las runas tienen un número fijo de "hijos" **children**. Los hijos pueden ser runas con hijis, y los programas de Hoon funcionan encadenándolos hasta que se obtiene un valor, no otra runa. Por esta razón, rara vez necesitamos cerrar expresiones. Tenga en cuenta este esquema cuando examine el código Hoon.

El propósito específico de la runa **|=** es crear una puerta (**gate**). Una puerta es lo que se llamaría una función en otros idiomas: toma una entrada, realiza un cálculo específico y luego produce una salida.

Debido a que sólo estamos en la línea 1, todo lo que estamos haciendo con la puerta está creando, y luego especificando qué tipo de entrada de la puerta lleva con el primer hijo de esa runa: end=@. La parte **end** de nuestro código es simplemente un nombre que le damos a la entrada del usuario para que podamos usar el número más tarde. **=@** significa que restringimos el tipo de entrada que nuestra puerta acepta al tipo de átomo, o **@** para abreviar. Un átomo es un número natural.

Nuestro programa es simple, por lo que todo el programa es la puerta que se está creando aquí. El resto de nuestras líneas de código son parte del segundo hijo de nuestra puerta y determinan cómo nuestra puerta produce una salida.

#### Línea 2
~~~
=/  count=@  1
~~~
Esta línea comienza con la runa **=/**, que almacena un valor con un nombre y especifica su tipo. Se necesitan tres hijos.

**count=@** (el primer hijo) almacena 1, (el segundo hijo) como count y  especifica que tiene es **@**

Estamos utilizando count para realizar un seguimiento de qué números estamos incluyendo en la lista que estamos construyendo. Lo usaremos más adelante en el programa.

#### Línea 3
~~~
| -
~~~
La runa **|-** funciona como un punto de "reinicio" para la recursión que se definirá más adelante. Se necesita un hijo.

#### Línea 4
~~~
^ -   (lista @ )
~~~
La runa **^-** restringe la salida a un cierto tipo. Se necesitan dos hijos.

En este caso, la runa especifica que la salida de nuestra puerta debe ser **(list @)**, es decir, una lista de átomos.

#### Lineas 5 y 6
~~~
?:  =(end count)
  ~
~~~
**?:** es una runa que evalúa si su primer hijo es verdadero o falso. Si ese hijo es verdadero, el programa se bifurca al segundo hijo. Si es falso, se ramifica al tercer hijo. **?:** lleva tres hijos.

**=(end count)** comprueba si la entrada del usuario es igual al valor count que estamos incrementando para construir la lista. Si estos valores son iguales, finalizamos el programa, porque nuestra lista ha sido desarrollada hasta donde debe estar. Tenga en cuenta que esta expresión es, de hecho, una expresión de runas, simplemente escrita de una manera diferente a la que ha visto hasta ahora. **=(end count)** es una forma irregular de **.=  end  count**, diferente en apariencia, pero idénticos en funcionamiento. **.=** es una runa que verifica la igualdad de sus dos hijos y produce un "verdadero" o "falso" en función de lo que encuentra.

**~** simplemente representa el valor "nulo". El programa se ramifica aquí si en la línea 5 encuentra que end es igual count. Las listas en Hoon siempre terminan con **~**, por lo que necesitamos que esto sea lo último que incluimos en nuestra lista.

#### Línea 7
**:-** es una runa que crea una celda , un par ordenado de dos valores, como [1 2]. Se necesitan dos hijos.

En nuestro caso, **:-  count** crea una celda de cualquier valor almacenado count, y luego con el producto de la línea 8.

#### Línea 8
~~~
$(count (add 1 count))
~~~
El código anterior es, una vez más, una forma compacta de escribir una expresión de runas. Todo lo que necesita saber es que esta línea de código reinicia el programa en **|-**, excepto con el valor almacenado en countincrementos de 1. La construcción de **(count (add 1 count))** le dice a la computadora, "reemplace el valor de **count** con **count** + 1".

Se dará cuenta de que usamos una palabra desconocida aquí: **add**. A diferencia de count y end, add no está definido en ninguna parte de nuestro programa. Eso es porque es una puerta que está predefinida en la biblioteca estándar de Hoon. La biblioteca estándar está llena de compuertas predefinidas que generalmente son útiles, y estas compuertas se pueden usar como algo que usted definió en su propio programa. Puede ver esta puerta y otros operadores matemáticos en la sección 1a de la documentación de la biblioteca estándar.

#### Explicación
Si no tiene claro cómo el programa está construyendo la lista, está bien.

Nuestro programa funciona haciendo que cada iteración de la lista cree una celda. En cada una de estas celdas, la cabeza (**head**), la primera posición de la celda, se llena con el valor de iteración actual de count. La cola (**tail**) de la celda, su segunda posición, se llena con el producto de una nueva iteración de nuestro código que comienza en **|-**. Esta iteración creará otra celda, cuyo encabezado se completará con el valor incrementado de count, y la cola de la cual comenzará otra iteración. Este proceso continúa hasta que **?:** se bifurca a **~** (nulo). Cuando eso sucede, la lista finaliza y el programa no tiene nada más que hacer, por lo que finaliza. Por lo tanto, una lista integrada de celdas anidadas se puede visualizar así:

   [1 [2 [3 [4 ~]]]]

~~~
         / \
        1   .
           / \
          2   .
             / \
            3   .
               / \
              4   ~
~~~
Si aún no intuye cómo funciona esto, no se preocupe. Analizaremos más profundamente la recursividad más adelante con el Tutorial de recursividad .