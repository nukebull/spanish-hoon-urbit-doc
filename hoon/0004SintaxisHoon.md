## Sintaxis de Hoon
El estudio de Hoon se puede dividir en dos partes: sintaxis y semántica.

La sintaxis de un lenguaje de programación es el conjunto de reglas que determinan qué cuenta como código admisible en ese lenguaje. Determina qué caracteres se pueden usar en la fuente y también cómo se pueden ensamblar estos caracteres para constituir un programa. Intentar ejecutar un programa que no siga estas reglas dará como resultado un error de sintaxis.

La semántica de un lenguaje de programación se refiere al significado de las diversas partes del código de ese lenguaje.

En esta lección daremos una visión general de la sintaxis de Hoon. Al final, debes estar familiarizado con todos los elementos básicos del código Hoon.

Personajes Hoon
Los archivos fuente de Hoon están compuestos casi en su totalidad por los caracteres ASCII imprimibles . Hoon no acepta ningún otro carácter en los archivos de origen, excepto UTF-8 en las cadenas entre comillas. Los caracteres de tabulación dura son ilegales; use dos espacios en su lugar.
~~~
> "You can put ½ in quotes, but not elsewhere!"
"You can put ½ in quotes, but not elsewhere!"

> 'You can put ½ in single quotes too.'
'You can put ½ in single quotes too.'

> "Some UTF-8: ἄλφα"
"Some UTF-8: ἄλφα"
~~~
Leyendo a Hoon en voz alta
El código Hoon hace un uso intensivo de símbolos no alfanuméricos. Leer el código en voz alta usando los nombres propios de los símbolos ASCII es tedioso, por lo que hemos mapeado las sílabas a los símbolos:
~~~
ace [1 space]       gap [>1 space, nl]  pat @
bar |               gar >               sel [
bas \               hax #               ser ]
buc $               hep -               sig ~
cab _               kel {               soq '
cen %               ker }               tar *
col :               ket ^               tic `
com ,               lus +               tis =
doq "               mic ;               wut ?
dot .               pal (               zap !
fas /               pam &
gal <               par )
~~~
Aprender y usar estos nombres de símbolos no alfanuméricos es completamente opcional; puedes convertirte en un programador experto de Hoon sin ellos. Sin embargo, muchos programadores de Hoon los encuentran útiles, particularmente cuando hablan con alguien más que los conoce.

Tenga en cuenta que la lista incluye dos formas separadas de espacios en blanco: acepara un solo espacio; gapes más de 2 espacios o una nueva línea. En Hoon, la única importancia del espacio en blanco es la distinción entre acey gap, es decir, la distinción entre un espacio y más de uno.

Expresiones de Hoon
Una expresión es una combinación de caracteres que el lenguaje interpreta y evalúa para producir un valor. Los programas de Hoon están compuestos completamente de expresiones.

Las expresiones de Hoon pueden ser básicas o complejas. Las expresiones básicas de Hoon son fundamentales, lo que significa que no se pueden dividir en expresiones más pequeñas. Las expresiones complejas están formadas por expresiones más pequeñas (que se denominan subexpresiones ).

Hay muchas categorías de expresiones Hoon: sustantivos literales, expresiones de ala, expresiones de tipo y expresiones de runas. Repasemos cada uno.

Literales sustantivos
Un sustantivo es un átomo o una célula. Un átomo es un entero sin signo y una celda es un par de sustantivos.

Hay expresiones literales para cada tipo de sustantivo. Un sustantivo literal es solo una notación para representar un valor nominal fijo.

Comenzamos con literales atómicos. Cada uno de estos es una expresión básica de Hoon que se evalúa a sí misma. Ejemplos:
~~~
> 1
1

> 123
123

> 123.456
123,456

> 'Hola'
'Hola'

> ~zod
~ zod
~~~
Recuerde de la Lección 1.2 que, aunque los átomos son enteros sin signo, pueden imprimirse de diferentes maneras. La forma en que se representará un átomo depende de su aura. La sintaxis literal para cada una de las auras codificadas se explicará más adelante en la Lección 2.1 .

Los literales de celda se pueden escribir en Hoon usando [ ]. Los literales de celda son complejos porque otras expresiones se colocan dentro de los corchetes. Ejemplos:
~~~
> [6 7]
[6 7]

> [[12 14] [654 123.456 980]]
[[12 14] 654 123.456 980]

> ['hello' 0xdad]
['hello' 0xdad]

> [123 [~zod ~ten] .1.2837]
[123 [~zod ~ten] .1.2837]
~~~
También puede poner expresiones complejas dentro de corchetes para hacer una celda. Estas son expresiones válidas, pero no son literales de celda, naturalmente:
~~~
> [(add 22 33) (mul 22 33)]
[55 726]
~~~
#### Alas (Wings)
Un **Ala** (**Wings**) es una serie de extremidades(**limb**) separadas por **.** Se puede encontrar una explicación más profunda en las páginas de referencia de las alas y las extremidades .

Comencemos con el caso basico: una sola extremidad (**limb**). Una (**limbs expression**) es una espresion de ala trivial: Son alas (**wings**) de un solo componente (**limb**)
~~~
+2
-
+>
&4
a
b
add
mul
~~~

Como miembro especial también tenemos **$**. Este es el nombre del brazo (**arm**) en núcleos (**cores**) especiales de un solo brazo  (**arm**)llamados "puertas"(**gates**). (Cubriremos el papel de **$** en la Lección 1.4 .)

Las expresiones de ala con múltiples extremidades son expresiones complejas. Ejemplos:
~~~
+2.+3.+4
-.+.+
resolution.path.into.subject
+>.$
a.b.c
-.b.+2
-.add
~~~
#### Expresiones de tipo
Hoon es un lenguaje estáticamente escrito. Aprenderá más sobre el sistema de tipos más adelante en este capítulo. Por ahora, solo sepa que el sistema de tipos de Hoon usa símbolos especiales para indicar ciertos tipos fundamentales: ~ (nulo **null**), * (sustantivo **noun**), @  (átomo **atom**), ^ (celda **cell**) y ? (bandera **flag**). Cada uno de estos símbolos se puede usar como una expresión independiente de Hoon. En el caso de **@** que pueda haber una serie de letras a continuación, para indicar un aura atómica; por ejemplo, @s, @rs, @tas, y @tD.

También pueden ser puestos entre paréntesis para indicar los tipos de compuesto, por ejemplo, [@ ^], [@ud @sb], [[? *] ^]. (Técnicamente, estas expresiones no siempre indican tipos compuestos. En ciertos contextos se interpretan de una manera diferente. Abordaremos esta variación de significado en la Lección 2.3 .)

#### Expresiones de runas
Una runa es solo un par de caracteres ASCII (un dígrafo). Que ya ha visto algunas runas en el Capítulo 1: **|=, |%, y |_**. Usualmente pronunciamos las runas como una combinando de nombres, por ejemplo: 'ket-hep' para **^-**, 'bar-tis' para **|=y** 'bar-cen' para **|%**. Como se indicó anteriormente, estos nombres son completamente opcionales.

Las expresiones con una runa al principio son expresiones de runa. La mayoría de las runas se usan al comienzo de una expresión compleja, pero hay excepciones. Por ejemplo, las runas **--** y **==** se usan al final de ciertas expresiones.

Las runas se clasifican por familia (con la excepción de **--** y **==**). El primero de los dos símbolos indica la familia, por ejemplo, la runa **^-** está en la familia **^** de las runas, y las runas **|=y |%** están en la familia **|**. Las runas de una familia en particular generalmente tienen significados relacionados. Dos ejemplos simples: las runas de la familia **|** se usan para crear núcleos (**cores**), y las runas de la familia **:** se usan para crear celdas(**cells**).

Las expresiones de runa suelen ser complejas, lo que significa que suelen tener una o más subexpresiones. La sintaxis apropiada varía de runa a runa; después de todo, se usan para diferentes propósitos. Para ver las reglas de sintaxis de una runa en particular, consulte la referencia de runas . Sin embargo, hay algunos principios generales que se aplican a todas las expresiones de runas.

Las runas generalmente no necesitan ser cerradas. En otros idiomas, verá una gran cantidad de terminadores, como abrir y cerrar paréntesis, y esta forma de hacerlo está ausente en gran medida de Urbit. Esto se debe a que todas las runas tienen un número fijo de hijos. Los hijos (**child**) pueden ser runas (con más hijos), y los programas de Hoon funcionan encadenando a través de estas series de niños hasta que se obtiene un valor, no otra runa. Esto hace que el código Hoon sea agradable y ordenado de ver.

#### Formas Altas (Tall) y Planas (Flat)
Hay dos formas de sintaxis de runas: altas y planas . La forma alta generalmente se usa para expresiones de varias líneas, y la forma plana se usa para expresiones de una línea. La mayoría de las runas se pueden usar en formas altas o planas. Las expresiones de forma alta pueden contener subexpresiones de forma plana, pero las expresiones de forma plana pueden no contener forma alta.

Las reglas de espaciado difieren en las dos formas. En forma alta, cada runa y subexpresión deben estar separadas de las demás por un salto (**gap**), es decir, por dos o más espacios, o un salto de línea. En forma plana, la runa es seguida inmediatamente por paréntesis **( )**, y las diversas subexpresiones dentro de los paréntesis deben estar separadas de las demás por un **ace**, es decir, por un solo espacio.

Ver un ejemplo lo ayudará a comprender la diferencia. La runa **:-** se usa para producir una célula. En consecuencia, le siguen dos subexpresiones: la primera define la cabeza de la celda y la segunda define la cola. Aquí hay tres formas diferentes de escribir una expresión **:-** en forma alta (**Tall**):
~~~
> :-  11  22
[11 22]

> :-  11
  22
[11 22]

> :-
  11
  22
[11 22]
~~~

Todos hacen lo mismo. El primer ejemplo muestra que, si lo desea, puede escribir código de formulario 'alto' en una sola línea. Observe que hay dos espacios entre la runa **:-** y 11, y también entre 11 y 22. Este es el espacio mínimo necesario entre las diversas partes de una expresión de forma alta: cualquier cantidad menor dará como resultado un error de sintaxis.

Por lo general, se usan uno o más saltos de línea para dividir una expresión de forma alta. Esto es especialmente útil cuando las subexpresiones son extensiones de código largas. La misma expresión **:-** en forma plana es:
~~~
> :-( 11 22)
[11 22]
~~~
Esta es la forma preferida de escribir una expresión en una sola línea. La runa en sí es seguida por un paréntesis, y las subexpresiones en el interior están separadas por un solo espacio. Cualquier espacio más que eso da como resultado un error de sintaxis.

Casi todas las expresiones de runas se pueden escribir en cualquier forma, pero hay excepciones. Las expresiones **|%y |_**, por ejemplo, solo se pueden escribir en forma alta. (De todos modos, son demasiado complicados para caber cómodamente en una línea).

#### Runas de anidamiento
Dado que las runas tienen un número fijo de hijos, uno puede visualizar cómo se construyen las expresiones Hoon al pensar en cada runa seguida de una serie de recuadros para ser llenados, uno para cada uno de sus hijos. Vamos a ilustrar esto con la runa **:-**.
~~~
    =====   =====
:-  =   =   =   =
    =====   =====
~~~

Aquí hemos dibujado la runa **:-** seguida de un cuadro para cada uno de sus dos hijos. Podemos llenar estos cuadros con un valor o una runa adicional. La siguiente figura corresponde a la expresión de Hoon:
~~~
 :-  2  3

    =====   =====
:-  = 2 =   = 3 =
    =====   =====
~~~


Esto, por supuesto, se evalúa en la célula [2 3].

La siguiente figura corresponde a la expresión de Hoon.
~~~
 :-  :-  2  3  4

    ====================
    =    =====  =====  =  ===== 
:-  = :- = 2 =  = 3 =  =  = 4 =
    =    =====  =====  =  =====
    ====================
~~~



Esto se evalúa como [[2 3] 4], y podemos pensar que el segundo **:-** está "anidado" dentro del primer **:-**.

¿A qué expresión de Hoon corresponde la siguiente figura y a qué se evalúa?
~~~
           =====================
    =====  =     =====  =====  =
:-  = 2 =  = :-  = 3 =  = 4 =  =
    =====  =     =====  =====  =
           =====================
~~~



Correcto. Esto representa la expresión de Hoon
~~~
 :-  2  :-  3  4
~~~
 Se evalúa como [2 [3 4]]. Sin embargo, recuerde que si ingresa esto en el dojo, se imprimirá como [2 3 4].

Pensar en términos de estos diagramas de "bloques LEGO", así como los diagramas de árbol binario más literales utilizados en la Lección 1.2 , puede ser una táctica útil de aprendizaje y depuración.

##### Ejercicio 1.3.a.
Dibuja árboles binarios correspondientes a las figuras anteriores. Determine una correspondencia uno a uno entre los cuadros anidados ordenados linealmente (es decir, lo que se muestra en las figuras anteriores) y los árboles binarios. Deberá hacer uso de la convención [a [b c]] = [a b c].

#### Formas irregulares
Algunas runas se usan con tanta frecuencia que tienen contrapartes irregulares que son más fáciles de escribir y que significan exactamente lo mismo. La sintaxis irregular de las runas es, por lo tanto, una forma de azúcar sintáctica . Toda la sintaxis de runas irregulares es plana. (De esto se deduce que todas las expresiones de forma alta (**Tall**) son regulares).

Veamos otro ejemplo. La runa **.=** toma dos subexpresiones, las evalúa y prueba la igualdad de los resultados. Si son iguales produce **%.y** "sí"; de lo contrario **%.n** para 'no'. En forma alta:
~~~
> .=  22  11
%.n

> .=  22
  (add 11 11)
%.y
~~~
Y en forma plana:
~~~
> .=(22 11)
%.n

> .=(22 (add 11 11))
%.y
~~~
La forma irregular de la runa **.=** es solo **=( )**:
~~~
> =(22 11)
%.n

> =(22 (add 11 11))
%.y
~~~
Los ejemplos anteriores tienen otra forma irregular: (add 11 11). Esta es la forma irregular de **%+**, que llama una puerta (**gate**) (es decir, una función Hoon) con dos argumentos para la muestra.
~~~
> %+  add  11  11
22

> %+(add 11 11)
22
~~~
La  sintaxis irregular **( )** de llamada de puerta (**gate-calling**) es versátil: también es un atajo para llamar a una puerta con un soloargumento, para lo que es la runa **%-**:
~~~
> (dec 11)
10

> %-  dec  11
10

> %-(dec 11)
10
~~~
La ( ) sintaxis de llamada de puerta se puede usar para puertas con cualquier número de argumentos.

Puede encontrar otras formas irregulares en el documento de referencia de expresiones irregulares.

#### Expresiones que solo son irregulares
Hay ciertas expresiones irregulares que no son azúcar sintáctica para equivalentes de forma regular, son únicamente irregulares. No hay mucho en general que se pueda decir sobre estos porque son todos diferentes, pero podemos ver algunos ejemplos.

A continuación se utiliza el **`** símbolo para crear una célula cuya cabeza es nula, **~**.
~~~
> `12
[~ 12]

> `[[12 14] 16]
[~ [12 14] 16]
~~~
Poner **,.** delante de una expresión de ala elimina una cara, si hay una.
~~~
> -:[a=[12 14] b=[16 18]]
a=[12 14]

> ,.-:[a=[12 14] b=[16 18]]
[12 14]

> ,.-:[[12 14] b=[16 18]]
[12 14]

> +:[a=[12 14] b=[16 18]]
b=[16 18]

> ,.+:[a=[12 14] b=[16 18]]
[16 18]
~~~
Para ver otras expresiones irregulares, consulte el documento de referencia de expresiones irregulares .

#### La biblioteca estándar
La biblioteca estándar de Hoon es una compilación de compuertas (funciones) de Hoon generalmente útiles. Ya has visto esto: en expresiones como **(add 11 11)**, addes una función de la biblioteca estándar.

Es importante saber acerca de las funciones estándar de la biblioteca, ya que hacen que ciertas tareas sean mucho más fáciles y evitan que tenga que escribir el código. Si no usó la función **add** de biblioteca, por ejemplo, tendría que escribir un código como este cada vez que quisiera encontrar la suma de dos números:
~~~
++  add
  ~/  %add
  |=  [a=@ b=@]
  ^-  @
  ?:  =(0 a)  b
  $(a (dec a), b +(b))
~~~
Las funciones de biblioteca estándar a menudo se crean con otras funciones de biblioteca estándar, pero en última instancia, esas funciones utilizadas solo se crean con runas. Observe cómo en el código anterior **add** se construye con la función **dec**, que disminuye un valor en 1.

#### Materiales de referencia
La sintaxis de Hoon puede ser intimidante para los no iniciados, por lo que es bueno recordar dónde se pueden encontrar las expresiones. La sección de referencia en sí misma es un buen lugar para encontrar los materiales de referencia que necesita. Es probable que estas secciones secundarias sean útiles:

1. La página Runas te mostrará cómo usar cualquier runa Hoon. 

2. La hoja de trucos es un lugar más compacto para buscar expresiones de runas.

3. La sección de la biblioteca estándar tiene sus subpáginas ordenadas por categoría. Entonces, las funciones aritméticas, por ejemplo, se encuentran en la misma página.

4. La Guía de estilo de Hoon le mostrará cómo escribir su código Hoon para que sea idiomático y fácil de entender para otros.

#### Depuración
Cuando tiene un error en su código Hoon, puede suceder una de dos cosas. O el código no se ejecuta en absoluto y obtiene un error (como nest-fail), o su código se ejecuta pero produce resultados incorrectos. La página de resolución de problemas es un buen recurso para descubrir cómo depurar su código.

Hay un par de runas útiles asociadas con la depuración:

**!:** (zapcol), si está escrito en la parte superior de un archivo Hoon, activa una pila de seguimiento de depuración completa. Es una buena práctica usar siempre que esté aprendiendo.

**~&** (sigpam) se usa para imprimir su argumento cada vez que se ejecuta ese argumento. Entonces, si quisieras ver cuántas veces se ejecutó tu programa foo, escribirías foo bar. Luego, cuando se ejecute su programa, se imprimirá fooen una nueva línea de salida cada vez que el programa lo encuentre por recursividad.

Pero hay más. Consulte la página de solución de problemas mencionada anteriormente para ver otras runas de depuración útiles y cómo usarlas.