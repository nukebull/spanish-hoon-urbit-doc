## 1.1 Configuración

Antes de comenzar a trabajar en Hoon, primero debe tener Urbit instalado e iniciar una nave (ship) que puede usar para probar ejemplos de Hoon. El aprendizaje interactivo es muy superior a la lectura pasiva.

### ¿Qué es un urbit?
Un urbit es una computadora virtual, con estado persistente que puede conectarse a la red Urbit. (Tenga en cuenta la 'u' minúscula aquí. 'Urbit' es la pila de software completa, mientras que 'una urbit' es una instancia local.) Cada urbit está asociado con un número único que desempeña tres roles distintos: (1) es una dirección en la red Urbit, (2) es una identidad criptográfica y (3) es (en principio) un nombre humano memorable. Normalmente, el nombre de un urbit se representa como una cadena que comienza con ~, como en ~zodo ~taglux-nidsep.

Puede que no parezcan números, pero lo son. Cada nombre de urbit se escribe en un formato base 256, donde cada 'dígito' es una sílaba. Imagine su número de teléfono como una cadena pronunciable que suena como un nombre en un idioma extranjero. Un urbit ordinario a nivel de usuario es un 'planeta', y se llama por un número de 32 bits. Este último se representa como una cadena de cuatro sílabas; por ejemplo, el nombre del planeta ~taglux-nidsepes el número 6.095.360.

### Instalando Urbit
Puede instalar Urbit en cualquier máquina Mac o Unix; si solo está probando Urbit o creando una nave de desarrollo, puede seguir los pasos para crear una nave de desarrollo aquí . En Windows, cree una máquina Linux virtual usando VirtualBox o una herramienta similar.

Una vez que hayas terminado, puedes arrancar tu propia nave. Si bien puede desarrollar en Hoon en su propia nave en la red en vivo, le sugerimos enfáticamente que desarrolle primero un barco de desarrollo.

### Empezando
Una vez que haya creado su nave de desarrollo, intentemos un comando básico. Escriba (add 2 2)en el indicador y presione "enter". Su pantalla ahora muestra:

~~~
fake: ~zod
ames: czar: ~zod on 31337 (localhost only)
http: live (insecure, public) on 80
http: live (insecure, loopback) on 12321
> (add 2 2)
4
~zod:dojo>
~~~

Que acaba de utilizar una función de la biblioteca estándar Hoon, add. A continuación, salga de Urbit con ctrl-d:

~~~
> (add 2 2)
4
~zod:dojo>
$
~~~

Su nave ya no se está ejecutando y está de vuelta en el indicador de terminal normal de su computadora. Si su nave es ~zod, entonces puede reiniciar la nave escribiendo:
~~~
Urbit zod
~~~

### Sustantivos
Ya ha utilizado una función de biblioteca estándar para producir un valor, en el dojo. Ahora que su nave está funcionando nuevamente, intentemos con otro. Ingrese el número 17.

No mostraremos el ~zod:dojo> aviso de aquí en adelante. Solo mostraremos el comando echo junto con su resultado.

Verás:
~~~
> 17
17
~~~

Le pediste al dojo que evaluara 17 y te repitió el número. Este valor es un 'sustantivo'. Hablaremos más sobre los sustantivos en la Lección 1.2 , pero primero escribamos un programa muy básico.

### Generadores
Los generadores son la forma más sencilla de escribir programas Hoon. Son un concepto en Arvo e implican guardar el código Hoon en un .hoonarchivo de texto. Si bien no son estrictamente parte del lenguaje Hoon, nos ocuparemos de los generadores a lo largo de este tutorial.

El tipo más simple de generador es el generador desnudo. Todos los generadores desnudos son puertas(gates): funciones que toman un argumento y producen una salida. Antes de crear un generador, debe montar tu eesxritorio para Unix, por lo tanto, para crear un generador, todo lo que necesita hacer, es escribir una puerta en un archivo (.hoon) y colocarlo en el directorio /home/gen de su nave. Suponiendo que ya haya montado (|mount /=home=). Después de esto, debe ejecutar (|commit %home) en dojo y su nave reconocerá el nuevo archivo. Para ejecutar un generador llamado mygen.hoon, escribirías +mygen argument el Dojo de tu nave.

~~~
dojo>|mount /=home=
"crear archivo mygen.hoon en /home/gen"
dojo>commit %home
dojo>+mygen argument
~~~

Si esto aún no tiene sentido, está bien. En la próxima lección, lo guiaremos a través de un ejemplo puerta que se ejecuta como generador.

### Editores de texto
La escritura de código en cualquier idioma generalmente se realiza mediante un editor de texto, pero los programas comunes como Notepad, Microsoft Word u OpenOffice Writer no son adecuados para la programación. En su lugar, querrá un editor de texto diseñado específicamente para escribir código. Esto ayudará con cosas como colorear palabras clave, encontrar patrones, paréntesis y paréntesis, etc.

A continuación se muestra una lista completa de editores de texto que admiten el resaltado de sintaxis de Hoon , una herramienta importante para cualquier lenguaje de programación. Para cada uno de estos editores, deberá descargar el editor y luego deberá instalar un paquete adicional de resaltador de sintaxis. El proceso para agregar soporte de Hoon difiere para cada editor. Para la mayoría de los editores de texto amigables para los novatos, esto se logra fácilmente desde el propio editor, y aprenderá cómo hacerlo siguiendo un tutorial para novatos para ese editor.

Si eres nuevo en programación, no pienses demasiado en qué editor usar; cualquiera de las opciones amigables para los novatos te conviene.

#### Editores de texto amigables para novatos
Estos editores son fáciles de usar para los codificadores por primera vez.

##### Atom
Atom es gratuito y de código abierto y se ejecuta en los principales sistemas operativos. Tlon mantiene un paquete para el soporte de Hoon y puede obtenerse usando el administrador de paquetes dentro del editor buscando Hoon.

##### Sublime
Sublime es de código cerrado, pero se puede descargar de forma gratuita y no hay un límite de tiempo obligatorio para la evaluación. Se ejecuta en todos los principales sistemas operativos.

##### Visual Studio Code
Visual Studio Code es gratuito y de código abierto y se ejecuta en todos los principales sistemas operativos. Se puede obtener soporte de Hoon en el menú Extensiones dentro del editor buscando Hoon.

#### Editores de texto avanzados
Estos editores de texto tienen una curva de aprendizaje alta y solo se recomiendan para programadores experimentados.

##### Emacs
Emacs es gratuito y de código abierto y se ejecuta en los principales sistemas operativos. El soporte de Hoon está disponible con hoon-mode.el .

##### Vim
Vim es gratuito y de código abierto y se ejecuta  en los principales sistemas operativos. El soporte de Hoon está disponible con hoon.vim y es mantenido por Tlon.