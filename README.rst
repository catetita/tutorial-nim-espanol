**Tutorial de Nim**
=========

.. image:: https://raw.githubusercontent.com/catetita/tutorial-nim-espanol/master/img.png

Introducción
--------------
Este documento es un tutorial para el lenguaje de programación Nim . Este tutorial asume que está familiarizado con los conceptos básicos de programación como variables, tipos o declaraciones, pero se mantiene muy básico. El manual contiene muchos más ejemplos de las características avanzadas del lenguaje. Todos los ejemplos de código en este tutorial, así como los que se encuentran en el resto de la documentación de Nim, siguen la guía de estilo de Nim .

El primer programa
--------------
Comenzamos el recorrido con un programa modificado "hola mundo":

.. code-block:: nim

 # This is a comment
 echo "What's your name? "
 var name: string = readLine(stdin)
 echo "Hi, ", name, "!" 

Elementos léxicos
--------------
Veamos los elementos léxicos de Nim con más detalle: al igual que otros lenguajes de programación, Nim consta de literales (de cadena), identificadores, palabras clave, comentarios, operadores y otros signos de puntuación.

**Literales de cuerdas y personajes**

* Los literales de cadena están encerrados entre comillas dobles; Literales de caracteres en comillas simples. Los caracteres especiales se escapan con ``\`` :`` \`` n significa nueva línea, ``\ t`` significa tabulador, etc. También hay literales de cadena en bruto 

.. code-block:: nim

 r"C:\program files\nim""


* En literales crudos, la barra invertida no es un personaje de escape.

La tercera y última forma de escribir literales de cadena son literales de cadena larga . Están escritos con tres citas:`` "" "..." "" ``; pueden abarcar varias líneas y el`` \`` no es un carácter de escape tampoco. Son muy útiles para incrustar plantillas de código HTML, por ejemplo.


**Comentarios**

Los comentarios comienzan en cualquier lugar fuera de una cadena o literal de caracteres con el ``# de`` carácter de hash . Los comentarios de documentación comienzan con ``##`` :

.. code-block:: nim

 # A comment.
 var myVariable: int ## a documentation comment


Los comentarios de documentación son tokens; solo se permiten en ciertos lugares en el archivo de entrada ya que pertenecen al árbol de sintaxis! Esta característica permite generadores de documentación más simples.

Los comentarios de varias líneas se inician con ``# [`` y terminan con ``] #`` . Los comentarios multilínea también pueden ser anidados.

.. code-block:: nim

 #[
 You can have any Nim code text commented
 out inside this with no indentation restrictions.
     yes("May I ask a pointless question?")
  #[
    Note: these can be nested!!
  ]#
 ]#

También puede usar la declaración de descarte junto con literales de cadena larga para crear comentarios de bloque:

.. code-block:: nim

 discard """ You can have any Nim code text commented
 out inside this with no indentation restrictions.
    yes("May I ask a pointless question?") """

**Números**

Los literales numéricos se escriben como en la mayoría de los otros idiomas. Como un giro especial,
se permiten guiones bajos para una mejor legibilidad: ``1_000_000`` (un millón). Un número que contiene un punto (o 'e' o 'E')
es un literal de punto flotante: ``1.0e9`` (mil millones). Los literales hexadecimales están prefijados con ``0x`` , 
los literales binarios con ``0b`` y los literales octales con ``0o`` . Un cero inicial solo no produce un octal. 

La sentencia **var**
--------------
La declaración var declara una nueva variable local o global:

.. code-block:: nim

 var x, y: int # declares x and y to have the type ``int``

Indentation can be used after the var keyword to list a whole section of variables:

.. code-block:: nim

 var
  x, y :int
  # a comment can occur here too
  a, b, c :string

La declaración de asignación
--------------
La declaración de asignación asigna un nuevo valor a una variable o, más generalmente, a una ubicación de almacenamiento:

.. code-block:: nim

 var x = "abc" # introduces a new variable `x` and assigns a value to it
 x = "xyz"     # assigns a new value to `x`

``=`` es el operador de asignación . El operador de asignación puede estar sobrecargado.
Puede declarar múltiples variables con una sola instrucción de asignación y todas las variables tendrán el mismo valor:

.. code-block:: nim

 var x, y = 3  # assigns 3 to the variables `x` and `y`
 echo "x ", x  # outputs "x 3"
 echo "y ", y  # outputs "y 3"
 x = 42        # changes `x` to 42 without changing `y`
 echo "x ", x  # outputs "x 42"
 echo "y ", y  # outputs "y 3"

Tenga en cuenta que la declaración de múltiples variables con una sola asignación que llama a un procedimiento puede tener resultados inesperados: el compilador desenrollará las asignaciones y terminará llamando al procedimiento varias veces. Si el resultado del procedimiento depende de los efectos secundarios, ¡sus variables pueden terminar teniendo valores diferentes! Para seguridad, utilice procedimientos libres de efectos secundarios si realiza múltiples tareas.

Constantes
--------------

Las constantes son símbolos que están vinculados a un valor. El valor de la constante no puede cambiar. El compilador debe poder evaluar la expresión en una declaración constante en tiempo de compilación:

.. code-block:: nim

 const x = "abc" # the constant x contains the string "abc"

La sangría se puede usar después de la palabra clave const para enumerar una sección completa de constantes:

.. code-block:: nim

 const
  x = 1
  # a comment can occur here too
  y = 2
  z = y + 5 # computations are possible

La declaración de *let*
--------------
La instrucción ``let`` funciona igual que la instrucción ``var`` , pero los símbolos declarados son variables de asignación única : después de la inicialización, su valor no puede cambiar:

.. code-block:: nim

 let x = "abc" # introduces a new variable `x` and binds a value to it
 x = "xyz"     # Illegal: assignment to `x`

La diferencia entre ``let`` y ``const`` es: ``permite`` introducir una variable que no se puede volver a asignar, ``const`` significa "imponer la evaluación del tiempo de compilación y colocarla en una sección de datos":

.. code-block:: nim

 const input = readLine(stdin) # Error: constant expression expected

.. code-block:: nim

 let input = readLine(stdin)   # works
 
Declaraciones de flujo de control
--------------

El programa de saludos consta de 3 instrucciones que se ejecutan de forma secuencial. Solo los programas más primitivos pueden salirse con la suya: también se necesitan ramificaciones y bucles.

**Si declaración**

La instrucción if es una forma de ramificar el flujo de control:

.. code-block:: nim

 let name = readLine(stdin)
 if name == "":
  echo "Poor soul, you lost your name?"
 elif name == "name":
  echo "Very funny, your name is name."
 else:
  echo "Hi, ", name, "!"

Puede haber cero o más partes ``elif`` , y la ``else`` parte es opcional.
La palabra clave ``elif`` es la abreviatura de ``else`` ``if`` , y es útil para evitar una sangría excesiva. 
(La "" es la cadena vacía. No contiene caracteres.)

**Declaración del caso**

Otra forma de ramificación es proporcionada por la declaración del caso. Una declaración de caso es una rama múltiple:

.. code-block:: nim

 let name = readLine(stdin)
 case name
 of "":
  echo "Poor soul, you lost your name?"
 of "name":
  echo "Very funny, your name is name."
 of "Dave", "Frank":
  echo "Cool name!"
 else:
  echo "Hi, ", name, "!"

Como se puede ver, para una ``of`` rama una coma separó la lista de valores también está permitido.

La declaración de caso puede tratar con enteros, otros tipos ordinales y cadenas. (Lo que un tipo ordinal es se explicará pronto). Para enteros u otros tipos de ordinales también son posibles rangos de valores:

.. code-block:: nim

 # this statement will be explained later:
 from strutils import parseInt

 echo "A number please: "
 let n = parseInt(readLine(stdin))
 case n
 of 0..2, 4..7: echo "The number is in the set: {0, 1, 2, 4, 5, 6, 7}"
 of 3, 8: echo "The number is 3 or 8"

Sin embargo, el código anterior no se compila: el motivo es que debe cubrir todos los valores que ``n`` puede contener, pero el código solo maneja los valores ``0..8`` . Dado que no es muy práctico enumerar todos los demás enteros posibles (aunque es posible gracias a la notación de rango), solucionamos esto indicando al compilador que por cada otro valor no se debe hacer nada:

.. code-block:: nim

 ...
 case n
 of 0..2, 4..7: echo "The number is in the set: {0, 1, 2, 4, 5, 6, 7}"
 of 3, 8: echo "The number is 3 or 8"
 else: discard

La declaración de descarte vacía es una declaración de no hacer nada . El compilador sabe que una declaración de caso con una parte else no puede fallar y, por lo tanto, el error desaparece. Tenga en cuenta que es imposible cubrir todos los valores de cadena posibles: es por eso que los casos de cadena siempre necesitan una rama ``else`` .

En general, la declaración de caso se usa para los tipos de subrango o enumeración donde el compilador comprueba que cubrió cualquier valor posible.

**Mientras declaración**

La instrucción while es una construcción de bucle simple:

.. code-block:: nim

 echo "What's your name? "
 var name = readLine(stdin)
 while name == "":
  echo "Please tell me your name: "
  name = readLine(stdin)
  # no ``var``, because we do not declare a new variable here

El ejemplo utiliza un bucle while para seguir preguntando a los usuarios por su nombre, siempre y cuando el usuario no escriba nada (solo presione RETORNO).

**Para declaración**

La instrucción ``for`` es una construcción para recorrer cualquier elemento que proporciona un iterador . El ejemplo utiliza el iterador incorporado de cuenta atrás :

.. code-block:: nim

 echo "Counting to ten: "
 for i in countup(1, 10):
  echo i
 # --> Outputs 1 2 3 4 5 6 7 8 9 10 on different lines

La variable ``i`` es declarada implícitamente por el bucle ``for`` y tiene el tipo ``int`` , porque eso es lo que devuelve el conteo . ``i`` corre a través de los valores 1, 2, .., 10. Cada valor es ``echo`` -ed. Este código hace lo mismo:

.. code-block:: nim

 echo "Counting to 10: "
 var i = 1
 while i <= 10:
  echo i
  inc(i) # increment i by 1
 # --> Outputs 1 2 3 4 5 6 7 8 9 10 on different lines

La cuenta regresiva se puede lograr con la misma facilidad (pero es menos necesaria):

.. code-block:: nim

 echo "Counting down from 10 to 1: "
 for i in countdown(10, 1):
  echo i
 # --> Outputs 10 9 8 7 6 5 4 3 2 1 on different lines

Desde contando ocurre tan a menudo en los programas, Nim también tiene un .. iterador que hace lo mismo:

.. code-block:: nim

 for i in 1..10:
  ...

El conteo de índice cero tiene dos accesos directos ``.. <`` y ``... ^`` para simplificar el conteo a uno menos que el índice más alto:

.. code-block:: nim

 for i in 0..<10:
  ...  # 0..9

o

.. code-block:: nim

 var s = "some string"
 for i in 0..<s.len:
  ...

Otros iteradores útiles para colecciones (como matrices y secuencias) son

* ``items`` y ``mitems`` , que proporciona elementos inmutables y mutables respectivamente, y
* ``pairs`` y ``mpairs`` que proporcionan el elemento y un número de índice (inmutable y mutable respectivamente)

.. code-block:: nim

 for index, item in ["a","b"].pairs:
  echo item, " at index ", index
 # => a at index 0
 # => b at index 1

**Los ámbitos y la declaración de bloque**

Las declaraciones de flujo de control tienen una característica aún no cubierta:
abren un nuevo ámbito. Esto significa que en el siguiente ejemplo, ``x`` no es accesible fuera del bucle:

.. code-block:: nim

 while false:
  var x = "hi"
 echo x # does not work

Una sentencia while (para) introduce un bloque implícito. Los identificadores solo son visibles dentro del bloque que han sido declarados.
La instrucción de ``block`` se puede usar para abrir un nuevo bloque explícitamente:

.. code-block:: nim

 block myblock:
  var x = "hi"
 echo x # does not work either

La etiqueta del bloque ( ``myblock`` en el ejemplo) es opcional.

**Declaración de ruptura**

Un bloque se puede dejar prematuramente con una instrucción break .
La instrucción ``break`` puede dejar un ``while`` , `` for``, o una instrucción de ``block`` .
Abandona la construcción más interna, a menos que se dé una etiqueta de un bloque:

.. code-block:: nim

 block myblock:
  echo "entering block"
  while true:
    echo "looping"
    break # leaves the loop, but not the block
  echo "still in block"

 block myblock2:
  echo "entering block"
  while true:
    echo "looping"
    break myblock2 # leaves the block (and the loop)
  echo "still in block"

**Continuar declaración**

Al igual que en muchos otros lenguajes de programación, una instrucción de ``continue`` comienza la siguiente iteración inmediatamente:

.. code-block:: nim

 while true:
  let x = readLine(stdin)
  if x == "": continue
  echo x

**Cuando declaración**

Ejemplo:

.. code-block:: nim

 when system.hostOS == "windows":
  echo "running on Windows!"
 elif system.hostOS == "linux":
  echo "running on Linux!"
 elif system.hostOS == "macosx":
  echo "running on Mac OS X!"
 else:
  echo "unknown operating system"


La instrucción ``when`` es casi idéntica a la instrucción ``if`` , pero con estas diferencias:

* Cada condición debe ser una expresión constante ya que es evaluada por el compilador.
* Las declaraciones dentro de una rama no abren un nuevo alcance.
* El compilador comprueba la semántica y produce código solo para las declaraciones que pertenecen a la primera condición que se evalúa como ``true`` .

La instrucción ``when`` es útil para escribir código específico de plataforma, similar a la construcción ``#ifdef`` en el lenguaje de programación C.


Declaraciones y sangría
--------

Ahora que cubrimos las declaraciones de flujo de control básico, volvamos a las reglas de sangría de Nim.

En Nim hay una distinción entre declaraciones simples y declaraciones complejas .
Las declaraciones simples no pueden contener otras declaraciones: la asignación, las llamadas a procedimientos o la declaración de devolución pertenecen a las declaraciones simples. 
Las declaraciones complejas como ``if``, ``when`` , ``for`` , ``while`` pueden contener otras declaraciones.
Para evitar ambigüedades, las declaraciones complejas siempre deben estar sangradas, pero las declaraciones simples y simples no:

.. code-block:: nim

 # no indentation needed for single assignment statement:
 if x: x = false
 
 # indentation needed for nested if statement:
 if x:
  if y:
    y = false
  else:
    y = true

 # indentation needed, because two statements follow the condition:
 if x:
  x = false
  y = false

Las expresiones son parte de una declaración que generalmente resulta en un valor. 
La condición en una sentencia if es un ejemplo para una expresión.
Las expresiones pueden contener sangría en ciertos lugares para una mejor legibilidad:

.. code-block:: nim

 if thisIsaLongCondition() and
    thisIsAnotherLongCondition(1,
       2, 3, 4):
  x = true

Como regla general, se permite la sangría dentro de las expresiones después de los operadores, un paréntesis abierto y después de las comas.

Con paréntesis y punto y coma ``(;)`` puede usar sentencias donde solo se permite una expresión:

.. code-block:: nim

 # computes fac(4) at compile time:
 const fac4 = (var x = 1; for i in 1..4: x *= i; x)


Procedimientos
---------

Para definir nuevos comandos como echo y readLine en los ejemplos, se necesita el concepto de un ``procedimiento`` . 
(Algunos idiomas los llaman métodos o funciones ). En Nim, los nuevos procedimientos se definen con la palabra clave ``proc`` :

.. code-block:: nim

 proc yes(question: string): bool =
  echo question, " (y/n)"
  while true:
    case readLine(stdin)
    of "y", "Y", "yes", "Yes": return true
    of "n", "N", "no", "No": return false
    else: echo "Please be clear: yes or no"

 if yes("Should I delete all your important files?"):
  echo "I'm sorry Dave, I'm afraid I can't do that."
 else:
  echo "I think you know what the problem is just as well as I do."

Este ejemplo muestra un procedimiento llamado sí que hace una pregunta al usuario y devuelve verdadero si contestó "sí" (o algo similar) y devuelve falso si respondió "no" (o algo similar).
Una declaración de retorno abandona el procedimiento (y, por lo tanto, el bucle while) inmediatamente.
La ( sintaxis : cadena): ``bool`` describe que el procedimiento espera un parámetro llamado pregunta de tipo ``cadena`` y devuelve un valor de tipo ``bool`` . 
El tipo bool está integrado: los únicos valores válidos para ``bool`` son ``true`` y ``false``. Las condiciones en las sentencias if o while deben ser de tipo ``bool ``.

Alguna terminología: en la pregunta de ejemplo se llama un parámetro (formal) , ``"Debería ..."`` se llama un argumento que se pasa a este parámetro.

**Variable de resultado**

Un procedimiento que devuelve un valor tiene una variable de ``result`` implícita declarada que representa el valor de retorno. Una declaración de ``return`` sin expresión es una abreviatura para el ``return`` ``result`` . El valor del ``result`` siempre se devuelve automáticamente al final de un procedimiento si no hay una declaración de ``return`` en la salida.

.. code-block:: nim

 proc sumTillNegative(x: varargs[int]): int =
  for i in x:
    if i < 0:
      return
    result = result + i

 echo sumTillNegative() # echos 0
 echo sumTillNegative(3, 4, 5) # echos 12
 echo sumTillNegative(3, 4 , -1 , 6) # echos 7

La variable de ``result`` ya está declarada implícitamente al inicio de la función, por lo que declararla de nuevo con 'var result', por ejemplo, la sombrearía con una variable normal del mismo nombre. La variable de resultado también ya está inicializada con el valor predeterminado del tipo. Tenga en cuenta que los tipos de datos referenciales serán ``nil`` al inicio del procedimiento y, por lo tanto, pueden requerir una inicialización manual.



**Parámetros**

Los parámetros son inmutables en el cuerpo del procedimiento. De forma predeterminada, su valor no se puede cambiar porque esto permite al compilador implementar el paso de parámetros de la manera más eficiente. Si se necesita una variable mutable dentro del procedimiento, debe declararse con ``var`` en el cuerpo del procedimiento. Es posible sombrear el nombre del parámetro, y en realidad un idioma:

.. code-block:: nim

 proc printSeq(s: seq, nprinted: int = -1) =
  var nprinted = if nprinted == -1: s.len else: min(nprinted, s.len)
  for i in 0 .. <nprinted:
    echo s[i]

Si el procedimiento necesita modificar el argumento para la persona que llama, se puede usar un parámetro ``var`` :

.. code-block:: nim

 proc divmod(a, b: int; res, remainder: var int) =
  res = a div b        # integer division
  remainder = a mod b  # integer modulo operation

 var
  x, y: int
 divmod(8, 5, x, y) # modifies x and y
 echo x
 echo y


En el ejemplo, ``res`` y ``remainder`` son ``var parameters`` . Los parámetros de la var pueden ser modificados por el procedimiento y los cambios son visibles para la persona que llama. Tenga en cuenta que el ejemplo anterior sería mejor utilizar una tupla como valor de retorno en lugar de usar los parámetros var.

**Declaración de descarte**

Para llamar a un procedimiento que devuelve un valor solo por sus efectos secundarios e ignorando su valor de retorno, se debe usar una declaración de ``discard`` . Nim no permite tirar silenciosamente un valor de retorno:

.. code-block:: nim

 discard yes("May I ask a pointless question?")

El valor de retorno se puede ignorar implícitamente si el proc / iterador llamado se ha declarado con el pragma ``discardable`` :

.. code-block:: nim

 proc p(x, y: int): int {.discardable.} =
  return x + y

 p(3, 4) # now valid

La ``discard`` de descarte también se puede usar para crear comentarios de bloque como se describe en la sección Comentarios .

**Argumentos con nombre**

A menudo, un procedimiento tiene muchos parámetros y no está claro en qué orden aparecen los parámetros. Esto es especialmente cierto para los procedimientos que construyen un tipo de datos complejo. Por lo tanto, los argumentos de un procedimiento se pueden nombrar, de modo que quede claro qué argumento pertenece a qué parámetro:

.. code-block:: nim

 proc createWindow(x, y, width, height: int; title: string;
                  show: bool): Window =
   ...

 var w = createWindow(show = true, title = "My Application",
                     x = 0, y = 0, height = 600, width = 800)


Ahora que usamos argumentos con nombre para llamar a ``createWindow``, el orden de los argumentos ya no importa. También es posible mezclar argumentos nombrados con argumentos ordenados, pero no es muy legible:

.. code-block:: nim

 var w = createWindow(0, 0, title = "My Application",
                     height = 600, width = 800, true)


El compilador comprueba que cada parámetro recibe exactamente un argumento.

**Valores predeterminados**

Para que el proceso ``createWindow`` sea ​​más fácil de usar, debe proporcionar ``default values`` ; estos son valores que se utilizan como argumentos si el llamante no los especifica:

.. code-block:: nim

 proc createWindow(x = 0, y = 0, width = 500, height = 700,
                  title = "unknown",
                  show = true): Window =
   ...

 var w = createWindow(title = "My Application", height = 600, width = 800)

Ahora la llamada a ``createWindow`` solo necesita establecer los valores que difieren de los valores predeterminados.

Tenga en cuenta que la inferencia de tipos funciona para parámetros con valores predeterminados; no hay necesidad de escribir ``title: string = "unknown"`` , por ejemplo.

**Procedimientos sobrecargados**

Nim proporciona la capacidad de sobrecargar procedimientos similares a C ++

.. code-block:: nim

 proc  toString ( x :  int ) :  string  =  ... 
 proc  toString ( x :  bool ) :  string  = 
  if  x :  result  =  "true" 
  else :  result  =  "false" 

 echo  toString ( 13 )    # llama al toString (x: int) proc 
 echo  toString ( true )  # llama al proceso toString (x: bool) proc


(Tenga en cuenta que ``toString`` suele ser el operador $ en Nim). El compilador elige el proceso más apropiado para las llamadas a ``toString`` . Aquí no se explica cómo funciona exactamente este algoritmo de resolución de sobrecarga (se especificará en el manual en breve). Sin embargo, no conduce a sorpresas desagradables y se basa en un algoritmo de unificación bastante simple. Las llamadas ambiguas se reportan como errores.

Los operadores
La biblioteca Nim hace un uso intensivo de la sobrecarga, una de las razones es que cada operador como + es solo un proceso sobrecargado. El analizador le permite usar operadores en ``infix notation (a + b)``  o ``prefix notation (+ a)`` . Un operador de infijo siempre recibe dos argumentos, un operador de prefijo siempre uno. (Los operadores de Postfix no son posibles, porque esto sería ambiguo: ``a @ @ b significa (a) @ (@b)`` o ``( a @ ) @ (b)`` ? Siempre significa ``(a) @ (@b)`` , porque no hay operadores de postfix en Nim.)

Aparte de unos cuantos incorporado operadores de palabras clave tales como y , o , no , los operadores siempre constan de los siguientes caracteres: ``+ - * \ / <> = @ $ ~ &%! ? ^. |``

Se permiten operadores definidos por el usuario. Nada le impide definir su propio operador ``@!? + ~`` , Pero hacerlo puede reducir la legibilidad.

La precedencia del operador está determinada por su primer carácter. Los detalles se pueden encontrar en el manual.

Para definir un nuevo operador, encierre el operador en backticks "` `":

.. code-block:: nim

 proc `$` (x: myDataType): string = ...
 # now the $ operator also works with myDataType, overloading resolution
 # ensures that $ works for built-in types just like before

La notación "` `" también se puede usar para llamar a un operador como cualquier otro procedimiento:

.. code-block:: nim
 
 if `==`( `+`(3, 4), 7): echo "True"


**Declaraciones hacia adelante**

Cada variable, procedimiento, etc. debe ser declarado antes de que pueda ser utilizado. (La razón de esto es que no es trivial evitar esta necesidad en un lenguaje que admita la programación meta tan ampliamente como lo hace Nim). Sin embargo, esto no se puede hacer para procedimientos recursivos mutuos:

.. code-block:: nim

 # forward declaration:
 proc even(n: int): boolproc odd(n: int): bool =

.. code-block:: nim

  assert(n >= 0) # makes sure we don't run into negative recursion
  if n == 0: false
  else:
    n == 1 or even(n-1)

 proc even(n: int): bool =
  assert(n >= 0) # makes sure we don't run into negative recursion
  if n == 1: false
  else:
    n == 0 or odd(n-1)


Aquí lo ``odd`` depende de ``even`` y viceversa. Por lo tanto, ``even`` es necesario introducirlo en el compilador antes de que esté completamente definido. La sintaxis para tal declaración de reenvío es simple: simplemente omita el ``=`` y el cuerpo del procedimiento. El ``assert`` solo agrega condiciones de borde y se cubrirá más adelante en la sección Módulos .

Las versiones posteriores del lenguaje debilitarán los requisitos para las declaraciones a plazo.

El ejemplo también muestra que el cuerpo de un proceso puede consistir en una sola expresión cuyo valor se devuelve implícitamente.


Iteradores
------

Volvamos al ejemplo de conteo simple:

.. code-block:: nim

 echo "Counting to ten: "
 for i in countup(1, 10):
  echo i

  ¿ Se puede escribir un proceso de conteo que admita este bucle? Intentemos:

.. code-block:: nim

  proc countup(a, b: int): int =
  var res = a
  while res <= b:
    return res
    inc(res)

Sin embargo, esto no funciona. El problema es que el procedimiento no solo debe regresar , sino que debe regresar y continuar después de que una iteración haya finalizado.
Este retorno y continuar se llama una declaración de rendimiento . Ahora lo único que queda por hacer es reemplazar la palabra clave proc por iterador y aquí está: 
nuestro primer iterador:

.. code-block:: nim

 iterator countup(a, b: int): int =
  var res = a
  while res <= b:
    yield res
    inc(res)

