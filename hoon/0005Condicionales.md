## Condicionales
En esta lección, escribiremos un generador que tome un número entero y verifique si es un número par entre 1 y 100. Esto ayudará a demostrar cómo funcionan las expresiones condicionales booleanas (verdaderas o falsas) en Hoon.
~~~
:-  %say
|=  [* [n=@ud ~] ~]
:-  %noun
^-  ?
?&  (gte n 1)
    (lte n 100)
    =(0 (mod n 2))
~~~

En la primera línea,
~~~
:-  %say
~~~

Comenzamos a crear un generador de la variante **%say**. El resultado de un generador **%say** es una celda (**cell**) con una cabeza (**head**) de **%say** y una cola que es una puerta  (**gate**). No es importante para comprender los condicionales; esto es solo código de plantilla. Para obtener más información sobre generadores **%say**, consulte la documentación de Generadores .
~~~
|=  [* [n=@ud ~] ~]
~~~

El código anterior construye una puerta. El primer argumento de la puerta es una celda proporcionada por Dojo que contiene información del sistema que no vamos a utilizar, por lo que usamos __*__ para indicar "cualquier sustantivo ". La siguiente celda son nuestros argumentos proporcionados al generador al invocarlos en dojo. Aquí solo queremos uno @ud con la cara n.
~~~
:-  %noun
~~~

Este código es la tercera línea de la **%say** "codigo repetitivo", y produce un cask con la cabeza de %noun. Podríamos usar cualquiera marca (**mark**) aquí, pero **%noun** es el tipo más genérico, capaz de adaptarse a cualquier dato.
~~~
^-  ?
~~~

Esta línea convierte la salida como a flag, que es un tipo cuyos valores son %.y y %.n representa "sí" y "no". Estos se comportan como valores booleanos.

Veamos el condicional.

~~~
?&  (gte n 1)
~~~

**?&** (pronunciado "wut-pam") toma una lista de expresiones Hoon, terminadas por **==**, que evalúan a **a** y **flag** devuelve el "AND" lógico de estos flags. La mayoría de las runas toman un número fijo de hijos, pero los pocos que no (como **?&**) terminan su lista de niños con una runa de terminación. En nuestro contexto, eso significa que si el producto de cada uno de los hijos de **?&** es **%.y**, entonces el producto de toda la expresión **?&** también lo es **%.y**. De lo contrario, el producto del condicional **?&** es **%.n**.

El primer hijo de **?&** es **(gte n 1)**. Es una buena práctica poner la primera prueba booleana de un condicional en la misma línea que el condicional como hemos hecho aquí. Esto utiliza la función de biblioteca estándar **gte** que significa "mayor o igual que". **(gte a b)** devuelve **%.y** si aes mayor o igual que b, y de lo contrario **%.n**.

A continuación tenemos:
~~~
    (lte n 100)
~~~
**lte** es la función de biblioteca estándar para "menor o igual que". **(lte a b)** devuelve **%.y** si a es menor que b, y de lo contrario **%.n**.

La última prueba booleana que tenemos es
~~~
    =(0 (mod n 2))
~~~
Esto verifica si **0** es igual a **(mod n 2)**, en otras palabras, verifica si **n** es par. Produce **%.y** si **n** es par y si **n** es impar **%.n**.

Es una buena práctica en Hoon poner líneas "más claras" en la parte superior y líneas "más pesadas" en la parte inferior, por lo que hemos puesto el **=(0 (mod n 2))** último en la lista de condicionales.

Finalmente tenemos un terminador.
~~~
==
~~~
Esto marca el final de la lista de hijos de **?&**.

#### Otras runas
Solo utilizamos una runa "wut" en nuestro tutorial, pero hay muchas otras. Aqui hay algunos ejemplos mas:

* **?:** (pronunciado "wut-pam") es la runa "wut" más simple. Se necesitan tres hijos, también llamados subexpresiones. El primer hijo es una prueba booleana, por lo que busca a **%.y** o a **%.n**. El segundo hijo es una rama de sí, que es a lo que llegamos si la prueba booleana mencionada anteriormente se evalúa **%.y**. El tercer hijo es una no rama (**no-branch**), a la que llegamos si la prueba booleana se evalúa **%.n**. Estas ramas pueden contener cualquier tipo de expresión Hoon, incluidas otras expresiones condicionales. En lugar de la expresión **?&** en nuestro %saygenerador anterior, podríamos haber escrito
~~~
?:  ?&  (gte n 1)
        (lte n 100)
        =(0 (mod n 2))
    ==
    %.y
%.n
~~~
Por supuesto, hacerlo sería innecesariamente ofuscante: mencionamos esto solo para ilustrar que estas dos expresiones de Hoon tienen el mismo producto.

* **?!** ("wut-zup") es el operador lógico "NO", que invierte el valor de verdad de su hijo único. En lugar de **(lte n 100)** en nuestro generador **%say** de arriba, podríamos haber escrito **?!  (gth n 100)**. Nuevamente, esto sería una mala práctica, solo presentamos esto como un ejemplo.

* **?@** Lleva tres hijos. Se ramifica sobre si su primer hijo es un átomo .

* **?^** Lleva tres hijos. Se ramifica sobre si su primer hijo es una célula.

* **?~** Lleva tres hijos. Se ramifica sobre si su primer hijo es nulo.

* **?|** lleva un número indefinido de hijos. Es el operador lógico "o". Comprueba si al menos uno de sus hijos es verdadero.

Para ver una lista exhaustiva de runas "wut", consulte la documentación de referencia sobre condicionales .