## 1.2 Sustantivos

En Urbit, cada pieza de datos es un sustantivo . Para entender a Hoon, primero hay que entender los sustantivos.

### Definición del sustantivo
Un sustantivo es un átomo o una célula. Un átomo es un número natural ("entero sin signo" en la jerga informática) de cualquier tamaño, incluido el cero. Una celda es un par ordenado de sustantivos, generalmente indicado entre corchetes alrededor de los sustantivos en cuestión; es decir, [a b] donde a y b son sustantivos.

Aquí hay algunos átomos:
~~~
0
87
325
~~~
Aquí hay algunas celdas:
~~~
[12 13]
[12 [487 325]]
[[12 13] [87 65]]
[[83 [1 17]] [[23 [32 64]] 90]]
~~~
Todo lo anterior son sustantivos.

### Sustantivos como árboles binarios
Un árbol binario tiene un solo nodo base, y cada nodo del árbol puede tener hasta dos nodos secundarios (pero no necesita tener ninguno). Un nodo sin hijos es una 'hoja'. Puede pensar en un sustantivo como un árbol binario cuyas hojas son átomos, es decir, enteros sin signo. Todos los nodos no hoja son células. Para visualizar esto, considere la siguiente representación del sustantivo [12 [17 45]]:
~~~
[12 [17 45]]
    .
   / \
  12  .
     / \
    17 45
~~~
Cada número es un átomo; Cada punto es una celda. Veamos otro ejemplo, esta vez de [[7 13] [87 65]], con los nodos de celda mostrados explícitamente:
~~~
  [[7 13] [87 65]]
         .
        / \
  [7 13] [87 65]
    .       .
   / \     / \
  7  13   87 65
~~~
Un átomo es un árbol trivial de un solo nodo; por ejemplo, 17.

### Átomos
Los átomos son enteros sin signo. Estos son a menudo representadas por el dojo como enteros sin signo en notación decimal, por ejemplo, 17.

Para estar seguros de que entendemos la notación atómica de Hoon, ingresemos algunos átomos en el dojo:
~~~
> 3
3
~~~
Parece sencillo.
~~~
> 32
32
~~~
¿Por qué no volver a intentarlo para asegurarte?
~~~
> 320
320
~~~
¿Podemos agregar otro cero?
~~~
> 320
~~~
¡No funcionó! Cuando intentaste escribir el siguiente cero y convertirte 320en 3200, el dojo realmente borró el 0y te emitió un pitido .

¿No es un átomo un entero sin signo de cualquier tamaño? Es. Pero la forma de representar el número de cuatro dígitos 3200es en realidad 3.200:
~~~
> 3.200
3.200

> (mul 32100)
3.200
~~~
Solo piense en ello como "3.200", escrito en castellano con un punto.

¿Por qué? Los números largos son difíciles de leer. Sabemos esto, por lo que agrupamos los dígitos en tres. Por alguna razón, la mayoría de los lenguajes de programación no utilizan esta maravillosa innovación. Hoon lo hace.

La notación inglesa para decimales es más común que la notación castellana. Desafortunadamente, los puntos son seguros para URL y las comas no. Por razones que se discutirán en una lección posterior, es bueno tener una sintaxis regular para átomos que sea segura para URL.

¿Por qué el dojo eliminó su cero?

dojo analiza la línea de comando a medida que escribe y rechaza cualquier carácter después de que se detiene el analizador. Esto evita que ingrese comandos o expresiones que no están bien formados.

### Auras
Los átomos no son más que enteros sin signo (números naturales), pero a menudo es útil representar un átomo de una manera diferente. Esto se hace con un aura . El aura de un átomo es metadatos tipo utilizados por Hoon para determinar cómo interpretar ese átomo. Responde a la pregunta: ¿Qué tipo de información es este átomo?

Usamos auras porque los datos no siempre se representan como un número decimal positivo. A veces queremos representar datos como binarios, como texto o como un número negativo. Las auras nos permiten usar este tipo de datos sin cambiar el átomo subyacente .

Todavía no explicaremos completamente las auras y sus diversos usos. Las auras se cubrirán más ampliamente en una lección posterior. Por ahora, veamos los diversos tipos de sintaxis literal asociados con algunas auras de átomos.

En informática, un "literal" es una expresión que representa y evalúa a un valor fijo. La sintaxis predeterminada de un literal de átomo es la notación decimal de estilo castellano, como se ve arriba. Veamos algunas otras sintaxis literales de átomos: binario sin signo, hexadecimal sin signo, decimal con signo, binario con signo y hexadecimal.
~~~
> 0b1001
0b1001

> 0b1001.1101
0b1001.1101

> 0x9d
0x9d

> 0xdead.beef
0xdead.beef

> --9
--9

> -79
-79

> --0b1001
--0b1001

> -0x9d
-0x9d
~~~
Todo lo anterior son átomos. El sustantivo subyacente de cada uno es solo un entero sin signo, pero cada uno está escrito en una sintaxis especial que indica a Hoon que el átomo se representará de una manera diferente. Para ver sus valores en la notación atómica predeterminada, puede decirle a Hoon que deseche la información del aura. Haga esto precediendo cada expresión anterior con `@`.
~~~
> `@` 0b1001
9 9

> `@`0b1001.1101
157

> `@` 0x9d
157

> `@`0xdead.beef
3.735.928.559

> `@` --9
18 años

> `@` -79
157

> `@` --0b1001
18 años

> `@` -0x9d
313
~~~
La `@`sintaxis se utiliza para 'emitir' un valor a un átomo sin procesar, es decir, un átomo sin aura. No se preocupe exactamente por lo que `@`significa todavía. Los moldes se explicarán con más detalle en una lección posterior. Por ahora, todo lo que necesita saber es que un elenco no cambia el valor subyacente del sustantivo; se usa aquí solo para cambiar cómo se imprime el valor en la salida del dojo.

### Identidades, cords y terms
Las **identidades** de Urbit como ~zody ~sorreg-namtyvtambién son átomos, pero del aura @p:
~~~
> ~zod
~ zod

> ~sorreg-namtyv
~ sorreg-namtyv

> `@` ~ zod
0

> `@ p`0
~ zod

> `@` ~sorreg-namtyv
5.702.400

> `@p`5.702.400
~sorreg-namtyv
~~~
Hoon permite el uso de átomos como cadenas (**Strings**). Las Strings que están codificadas como átomos se llaman cordones (**cords**) . Los cordones son del aura @t. La sintaxis literal de un cordón es texto dentro de un par de comillas simples, por ejemplo, 'Hello world!'.
~~~
> 'Howdy!'
'Howdy!'

> `@`'Howdy!'
36.805.260.308.296

> 'Hello world!'
'Hello world!'

> `@`'Hello world!'
10.334.410.032.606.748.633.331.426.632
~~~
Hoon también tiene términos (**terms**) , del aura @tas. Los términos son valores constantes que se utilizan para etiquetar datos utilizando el sistema de tipos. Estas son cadenas (**strings**) precedidas por % y compuestas de letras minúsculas, números y guiones, es decir, 'kebab case'. El primer carácter después del % debe ser una letra. Por ejemplo, %a, %hello, %this-is-kebab-case123.
~~~
> %howdy
%howdy

> %this-is-kebab-case123
%this-is-kebab-case123

> `@`%howdy
521.376.591.720

> `@`'howdy'
521.376.591.720

> `@`%this-is-kebab-case123
74.823.134.616.534.770.182.309.416.397.866.674.543.716.995.786.868
~~~
### Lista de auras
Aquí hay una lista no exhaustiva de auras, junto con ejemplos de sintaxis literal correspondiente:
~~~
Aura Significado Ejemplo de sintaxis literal
-------------------------------------------------- -----------------------
@d fecha
  @da fecha absoluta ~ 2018.5.14..22.31.46..1435
  @dr fecha relativa (es decir, intervalo de tiempo) ~ h5.m30.s12
@ nil ~
@p base fonémica (nombre de urbit) ~ sorreg-namtyv
@r IEEE de punto flotante
  @rd doble precisión (64 bits). ~ 6.02214085774e23
  @rh media precisión (16 bits). ~~ 3.14
  @rq precisión cuádruple (128 bits). ~~~ 6.02214085774e23
  @rs precisión simple (32 bits) .6.022141e23
@s entero con signo, signo poco bajo
  @sb binario con signo --0b11.1000
  @sd decimal con signo --1.000.056
  @sv firmado base32 -0v1df64.49beg
  @sw base64 firmado --0wbnC.8haTg
  @sx hexadecimal firmado -0x5f5.e138
@t UTF-8 texto (cable) 'howdy'
  @ta texto ASCII (nudo) ~ .howdy
    @tas Símbolo de texto ASCII (término)% howdy
@u entero sin signo
  @ub binario sin signo 0b11.1000
  @ud decimal sin signo 1.000.056
  @uv unsigned base32 0v1df64.49beg
  @uw base64 sin firmar 0wbnC.8haTg
  @ux hexadecimal sin signo 0x5f5.e138
~~~  
### Casting a otras auras
Puede obligar a Hoon a interpretar un átomo de manera diferente utilizando los símbolos de aura en la tabla anterior; por ejemplo, `@ux` para hexadecimal sin signo, `@ub` para binario sin signo, etc .:
~~~
> `@ux`157
0x9d

> `@ub`157
0b1001.1101

> `@ub`0x9d
0b1001.1101

> `@ux`0b1001.1101
0x9d
~~~
Los átomos en los ejemplos anteriores tienen exactamente el mismo valor. Solo se interpretan e imprimen de manera diferente, dependiendo del aura.

Aprenderá más sobre átomos y auras en la Lección 2.1 .

### Células
No hay mucho misterio sobre las células. La izquierda de una celda se llama cabeza (**head**), y la derecha se llama cola (**tail**) . Las celdas se representan típicamente en Hoon con corchetes alrededor de un par de sustantivos.
~~~
> [32 320]
[32 320]
~~~
En esta celda 32 está la cabeza y 320 es la cola. Las células pueden contener células y átomos de otras auras también:
~~~
> [%hello 'world!']
[%hello 'world!']

> [[%in 0xa] 'cell']
[[%in 0xa] 'cell']
~~~
Dije que no hay mucho misterio sobre las células, pero sí un poco. Hasta ahora, cada vez que ingresamos una celda en el dojo, no solo devuelve la misma celda; la celda se imprimió de la misma manera que se ingresó. Esto no siempre sucede. El dojo está obligado a evaluar la entrada y devolver la respuesta correcta, pero no tiene que mostrar el resultado exactamente de la misma manera:
~~~
> [6 [62 620]]
[6 62 620]

> [6 62 620]
[6 62 620]

> [[6 62] 620]
[[6 62] 620]
~~~
En el primer ejemplo, el par interno de corchetes se cae. ¿Por qué pasó esto? ¿Por qué [6 62 620] se acepta como una célula a pesar de tener aparentemente más de dos átomos en ella?

De hecho, [6 [62 620]] y [6 62 620] se consideran equivalentes en Hoon. Una celda debe ser un par, lo que significa que siempre tiene exactamente dos sustantivos. Siempre que se omiten los corchetes de las celdas, de modo que visualmente parece haber más de dos sustantivos secundarios, se entiende implícitamente que los sustantivos más a la derecha constituyen una celda. Es decir, se supone que la cola de toda la celda es una celda de los sustantivos más a la derecha.

Esto es por qué [6 [62 620]] y [6 62 620] son equivalentes. [6 [62 620]] y [[6 62] 620], por otro lado, no son equivalentes. Si los ves como árboles binarios, puedes ver la diferencia:
~~~
 [6 [62 620]] [[6 62] 620]
     .             .
    / \           / \
   6   .         .  620
      / \       / \
     62 620    6  62
~~~
Cuando el dojo imprime una celda de forma bonita (dojo pretty-prints), siempre elimina los corchetes superfluos:
~~~
> [[12 13] [[14 15] [16 17]]]
[[12 13] [14 15] 16 17]
~~~

***Sin embargo, siempre debe recordar que aunque las celdas se pueden presentar como listas de más de dos sustantivos, siempre son solo pares.***

##### Ejercicio 1.2a
Sin usar el dojo, evalúe si cada uno de los siguientes pares de sustantivos es equivalente. (Las respuestas están al final de la página).
~~~
1. [[1 3] [6 2]] [1 3 [6 2]]
2. [[[[17 18] 20] 19 21] [[[17 18] 20] [19 21]]
3. [[[[[12 13] 14] 15] 16] 17] [12 13 14 15 16 17]
4. [12 [13 [14 [15 [16 17]]]]] [12 13 14 15 16 17]
5. [37 [99 17] 83] [37 [[99 17] 83]]
~~~
#### Direcciones de sustantivos
Los sustantivos tienen una estructura regular que puede ser explotada para dar una dirección única a cada uno de sus 'subnombres'. Un subnombre es una parte de un sustantivo que es en sí mismo un sustantivo; por ejemplo, [6 2] es un subnombre de [[1 3] [6 2]]. También podemos llamar a estos 'fragmentos sustantivos'.

¿Qué pasa si quieres referirte a un fragmento de sustantivo, y no a todo? Por ejemplo, ¿qué pasa si desea hacer referencia a la [6 2]parte de [[1 3] [6 2]]? Una forma es usar la dirección única de ese fragmento de sustantivo.

Para definir direcciones de sustantivos usaremos recursividad. El caso base es trivial: la dirección 1 de un sustantivo es el sustantivo completo. El caso generador: si el sustantivo en la dirección nes una celda, entonces su cabeza está en la dirección 2ny su cola está en la dirección 2n + 1. Por ejemplo, si la dirección 5 de algún sustantivo es una celda, entonces su cabeza está en la dirección 10 y su dirección de cola 11.

Dirección 1 del sustantivo [[1 3] [6 2]] es todo el sustantivo: [[1 3] [6 2]]. La cabeza del sustantivo en la dirección 1, [1 3], está en la dirección 2 x 1, es decir, 2. La cola del sustantivo en la dirección 1, [6 2], está en la dirección (2 x 1) + 1, es decir, 3.

Si la forma en que funciona no está clara de inmediato, recuerde que cada sustantivo puede entenderse como un árbol binario. El siguiente diagrama muestra la dirección de varios nodos del árbol:
~~~
          1
         / \
        2   3
       / \ / \
      4  5 6  7
             / \
            14 15
~~~
Hagamos algunos ejemplos en el dojo. Vamos a usar el slotoperador,, +para devolver fragmentos de un sustantivo. Para cualquier número entero sin signo n, +nevalúa el fragmento en la dirección n.
~~~
> +1:[22 [33 44]]
[22 33 44]

> +2:[22 [33 44]]
22

> +3:[22 [33 44]]
[33 44]

> +6:[22 [33 44]]
33

> +7:[22 [33 44]]
44
~~~
Notarás que usamos el :símbolo entre la sintaxis de la ranura y el sustantivo. No te preocupes por lo que esto significa todavía; Lo explicaremos en otra lección. Por ahora puedes leerlo como la palabra "de": +7:[22 [33 44]] es "ranura 7 de [22 [33 44]]".

¿Qué sucede si pides un fragmento que no existe?
~~~

> +2:[['apple' %pie] [0b1101 0xdad]]
['apple' %pie]

> +5:[['apple' %pie] [0b1101 0xdad]]
%pie

> +2:[[%one %two] [%three %four] [%five %six]]
[%one %two]

> +3:[[%one %two] [%three %four] [%five %six]]
[[%three %four] %five %six]

> +15:[[%one %two] [%three %four] [%five %six]]
%six
~~~
Para aquellos que prefieren pensar en términos de números binarios y árboles binarios, hay otra forma (equivalente) de entender el direccionamiento de sustantivos. Cuando la dirección del sustantivo se expresa como un número binario, puede pensar que el número indica una ruta de árbol desde el nodo superior.

Como antes, la raíz del árbol binario (es decir, el nombre completo) está en la dirección 1. Para el nodo de un árbol en cualquier dirección b, donde b hay un número binario, obtiene la dirección de su cabeza concatenando 0 a al final de b; y para obtener su cola, concatene a 1 hasta el final. Por ejemplo, la cabeza del nodo en la dirección binaria 111 está en 1110, y la cola está en 1111.

La dirección 1110 indica "la cabeza de la cola de la cola del sustantivo".

##### Ejercicio 1.2b
Sin usar el dojo, evalúe las siguientes expresiones. (Las respuestas están al final de la página).
~~~
1. +6: [[34 54] [86 48]]
2. +3: [13 [[[65 35] 54] 77]]
3. +12: [13 [[[65 35] 54] 77]]
4. +7: [13 [[[65 35] 54] 77]]
5. +10: [[[2 4 5] [3 6]] 11]
~~~
##### Ejercicio 1.2c
Sin usar el dojo, para cada fragmento a continuación, proporcione la dirección relativa al sustantivo [51 52 53 54 55]. (Las respuestas están al final de la página).
~~~
1. 51
2. 52
3. 53
4. [54 55]
5. 55
~~~
Eso es todo por nuestra introducción básica a los sustantivos. Presiona ctrl-dpara salir de tu nave, o pasa a la siguiente lección.

##### Ejercicio 1.2d
Dibuje un diagrama de árbol binario para cada uno de los siguientes sustantivos. Intenta hacer esto sin usar el dojo para simplificar las expresiones.
~~~
1. [[2 3] 4 [5 6 7]]
2. [[[5 6 7] 8 9] 10 11 [12 13]]
~~~
##### Ejercicio 1.2e
Escribe el siguiente árbol binario como sustantivo.
~~~
       .
      / \
     /   \
    .     .
   / \   / \
  .   3 4   .
 / \       / \
1   2     .   7
         / \
        5   6
~~~
#### Respuestas al ejercicio
##### 1.2a
~~~
No
si
No
si
si
~~~
##### 1.2b
~~~
86
[[[65 35] 54] 77]
[65 35]
77
3
~~~
##### 1.2c
~~~
+2
+6
+14
+15
+31
~~~
##### 1.2d
1. Simplifica la expresión
~~~
> [[2 3] 4 [5 6 7]]
[[2 3] 4 5 6 7]
~~~
que tiene el siguiente árbol:
~~~
      .
     / \
    /   \
   .     .
  / \   / \  
 2   3 4   .
          / \
         5   .
            / \
           6   7
~~~           
2. Simplifica la expresión
~~~
> [[[5 6 7] 8 9] 10 11 [12 13]]
[[[5 6 7] 8 9] 10 11 12 13]
~~~
que tiene el siguiente árbol:
~~~
          .
         / \
        /   \
       /     \
      .       .
     / \     / \
    /   .   10  .
   /   / \     / \  
  .   8   9   11  .
 / \             / \
5   .           12 13
   / \
  6   7
~~~
##### 1.2e
~~~
[[[1 2] 3] 4 [5 6] 7]
~~~