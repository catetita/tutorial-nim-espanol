**Tutorial de Nim**
===================

.. image:: https://raw.githubusercontent.com/catetita/tutorial-nim-espanol/master/img.png

Introducción
------------

Este documento es un tutorial para el lenguaje de programación Nim.
Este tutorial asume que está familiarizado con los conceptos básicos de programación,
como variables, tipos o declaraciones, pero se mantiene muy básico.
El manual contiene muchos más ejemplos de las características avanzadas del lenguaje.
Todos los ejemplos de código en este tutorial,
así como los que se encuentran en el resto de la documentación de Nim,
siguen la guía de estilo de Nim .

El primer programa
------------------

Comenzamos el recorrido con un programa modificado "hola mundo":

.. code-block:: nim

  # This is a comment
  echo "What's your name? "
  var name: string = readLine(stdin)
  echo "Hi, ", name, "!"

Elementos léxicos
-----------------

Veamos los elementos léxicos de Nim con más detalle:
al igual que otros lenguajes de programación, Nim consta de literales (de cadena),
identificadores, palabras clave, comentarios, operadores y otros signos de puntuación.

**Literales de cuerdas y personajes**

* Los literales de cadena están encerrados entre comillas dobles;
Literales de caracteres en comillas simples.
Los caracteres especiales se escapan con ``\``,
``\n`` significa nueva línea, ``\t`` significa tabulador, etc.
También hay literales de cadena en bruto.

.. code-block:: nim

  r"C:\program files\nim""


* En literales crudos, la barra invertida no es un personaje de escape.

La tercera y última forma de escribir literales de cadena son literales de cadena larga.
Están escritos con tres citas: ``"""..."""``;
Pueden abarcar varias líneas y el``\`` no es un carácter de escape tampoco.
Son muy útiles para incrustar plantillas de código HTML, por ejemplo.


**Comentarios**

Los comentarios comienzan en cualquier lugar fuera de una cadena o literal de caracteres con el ``#`` de carácter de hash.
Los comentarios de documentación comienzan con ``##``:

.. code-block:: nim

  # A comment.
  var myVariable: int ## a documentation comment

Los comentarios de documentación son tokens;
Solo se permiten en ciertos lugares en el archivo de entrada ya que pertenecen al árbol de sintaxis!
Esta característica permite generadores de documentación más simples.

Los comentarios de varias líneas se inician con ``#[`` y terminan con ``]#``.
Los comentarios multilínea también pueden ser anidados.

.. code-block:: nim

  #[
    You can have any Nim code text commented
    out inside this with no indentation restrictions.
      yes("May I ask a pointless question?")
    #[
      Note: these can be nested!!
    ]#
  ]#


**Números**

Los literales numéricos se escriben como en la mayoría de los otros idiomas.
Como un giro especial, se permiten guiones bajos para una mejor legibilidad:
``1_000_000`` (un millón).
Un número que contiene un punto (o 'e' o 'E') es un literal de punto flotante:
``1.0e9`` (mil millones).
Los literales hexadecimales están prefijados con ``0x``,
los literales binarios con ``0b`` y los literales octales con ``0o``.
Un cero inicial solo no produce un octal.

La sentencia **var**
--------------------

La declaración ``var`` declara una nueva variable local o global:

.. code-block:: nim

  var x, y: int  # declares x and y to have the type ``int``

La Indentacion puede ser usada luego de ``var`` para agrupar un conjunto de variables:

.. code-block:: nim

  var
    x, y :int
    # a comment
    a, b, c :string

La declaración de asignación
----------------------------

La declaración de asignación asigna un nuevo valor a una variable o,
más generalmente, a una ubicación de almacenamiento:

.. code-block:: nim

  var x = "abc" # introduces a new variable `x` and assigns a value to it
  x = "xyz"     # assigns a new value to `x`

``=`` es el operador de asignación .
El operador de asignación puede estar sobrecargado.
Puede declarar múltiples variables con una sola instrucción de asignación y
todas las variables tendrán el mismo valor:

.. code-block:: nim

  var x, y = 3  # assigns 3 to the variables `x` and `y`
  echo "x ", x  # outputs "x 3"
  echo "y ", y  # outputs "y 3"
  x = 42        # changes `x` to 42 without changing `y`
  echo "x ", x  # outputs "x 42"
  echo "y ", y  # outputs "y 3"

Tenga en cuenta que la declaración de múltiples variables con una sola
asignación que llama a un procedimiento puede tener resultados inesperados:
El compilador desenrollará las asignaciones y terminará llamando al procedimiento varias veces.
Si el resultado del procedimiento depende de los efectos secundarios,
¡sus variables pueden terminar teniendo valores diferentes!.
Para seguridad, utilice procedimientos libres de efectos secundarios si realiza múltiples tareas.

Constantes
----------

Las constantes son símbolos que están vinculados a un valor.
El valor de la constante no puede cambiar.
El compilador debe poder evaluar la expresión en una declaración constante en tiempo de compilación:

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
-----------------------

La instrucción ``let`` funciona igual que la instrucción ``var``,
pero los símbolos declarados son variables de asignación única:
después de la inicialización, su valor no puede cambiar:

.. code-block:: nim

  let x = "abc" # introduces a new variable `x` and binds a value to it
  x = "xyz"     # Illegal: assignment to `x`

La diferencia entre ``let`` y ``const`` es:
Permite introducir una variable que no se puede volver a asignar,
``const`` significa "imponer la evaluación del tiempo de compilación y colocarla en una sección de datos":

.. code-block:: nim

  const input = readLine(stdin) # Error: constant expression expected

.. code-block:: nim

  let input = readLine(stdin)   # works

Declaraciones de flujo de control
---------------------------------

El programa de saludos consta de 3 instrucciones que se ejecutan de forma secuencial.
Solo los programas más primitivos pueden salirse con la suya:
También se necesitan ramificaciones y bucles.

**if declaración**

La instrucción ``if`` es una forma de ramificar el flujo de control:

.. code-block:: nim

  let name = readLine(stdin)
  if name == "":
    echo "Poor soul, you lost your name?"
  elif name == "name":
    echo "Very funny, your name is name."
  else:
    echo "Hi, ", name

Puede haber cero o más partes ``elif``, y la ``else`` parte es opcional.
La palabra clave ``elif`` es la abreviatura de ``else`` ``if``,
y es útil para evitar una sangría excesiva.
(La ``""`` es la cadena vacía. No contiene caracteres.)

**case Declaración**

Otra forma de ramificación es proporcionada por la declaración de ``case``.
Una declaración de caso es una rama múltiple:

.. code-block:: nim

  let name = readLine(stdin)
  case name
  of "":
    echo "Poor soul, you lost your name?"
  of "name":
     "Very funny, your name is name."
  of "Dave", "Frank":
    echo "Cool name!"
  else:
    echo "Hi, ", name

Como se puede ver, para una ``of`` rama una coma separó la lista de valores también está permitido.

La declaración de caso puede tratar con enteros, otros tipos ordinales y cadenas.
(Lo que un tipo ordinal es se explicará pronto).
Para enteros u otros tipos de ordinales también son posibles rangos de valores:

.. code-block:: nim

  # this statement will be explained later:
  from strutils import parseInt

  echo "A number please: "
  let n = parseInt(readLine(stdin))
  case n
  of 0..2, 4..7: echo "The number is in the set: {0, 1, 2, 4, 5, 6, 7}"
  of 3, 8: echo "The number is 3 or 8"

Sin embargo, el código anterior no se compila:
Emotivo es que debe cubrir todos los valores que ``n`` puede contener,
pero el código solo maneja los valores ``0..8``.
Dado que no es muy práctico enumerar todos los demás enteros posibles
(aunque es posible gracias a la notación de rango),
solucionamos esto indicando al compilador que por cada otro valor no se debe hacer nada:

.. code-block:: nim

  case n
  of 0..2, 4..7: echo "The number is in the set: {0, 1, 2, 4, 5, 6, 7}"
  of 3, 8: echo "The number is 3 or 8"
  else: discard

La declaración de ``discard`` vacía es una declaración de no hacer nada.
El compilador sabe que una declaración de caso con una parte else no puede fallar y,
por lo tanto, el error desaparece.
Tenga en cuenta que es imposible cubrir todos los valores de cadena posibles:
Es por eso que los casos de cadena siempre necesitan una rama ``else``.

En general, la declaración de caso se usa para los tipos de subrango o
enumeración donde el compilador comprueba que cubrió cualquier valor posible.

**while declaración**

La instrucción while es una construcción de bucle simple:

.. code-block:: nim

  echo "What's your name? "
  var name = readLine(stdin)
  while name == "":
    echo "Please tell me your name: "
    name = readLine(stdin)
    # no ``var``, because we do not declare a new variable here

El ejemplo utiliza un bucle while para seguir preguntando a los usuarios por su nombre,
siempre y cuando el usuario no escriba nada (solo presione RETORNO).

**for declaración**

La instrucción ``for`` es una construcción para recorrer cualquier elemento que proporciona un iterador.
El ejemplo utiliza el iterador incorporado de cuenta atrás:

.. code-block:: nim

  echo "Counting to ten: "
  for i in countup(1, 10):
    echo i
  # --> Outputs 1 2 3 4 5 6 7 8 9 10 on different lines

La variable ``i`` es declarada implícitamente por el bucle ``for`` y
tiene el tipo ``int``, porque eso es lo que devuelve el conteo.
``i`` corre a través de los valores 1, 2, 3, 4, 5, 6, 7, 8, 9, 10.
Cada valor es mostrado con ``echo``. Este código hace lo mismo:

.. code-block:: nim

  echo "Counting to 10: "
  var i = 1
  while i <= 10:
    echo i
    inc i  # increment i by 1
  # --> Outputs 1 2 3 4 5 6 7 8 9 10 on different lines

La cuenta regresiva se puede lograr con la misma facilidad (pero es menos necesaria):

.. code-block:: nim

  echo "Counting down from 10 to 1: "
  for i in countdown(10, 1):
    echo i
  # --> Outputs 10 9 8 7 6 5 4 3 2 1 on different lines

Desde contando ocurre tan a menudo en los programas,
Nim también tiene un ``..`` iterador que hace lo mismo:

.. code-block:: nim

  for i in 1..10:
    echo i

El conteo de índice cero tiene dos accesos directos ``..<`` y ``..^``
para simplificar el conteo a uno menos que el índice más alto:

.. code-block:: nim

  for i in 0..<10:
    echo i  # 0..9

o

.. code-block:: nim

  var s = "some string"
  for i in 0..<s.len:
    echo i

Otros iteradores útiles para colecciones (como matrices y secuencias) son:

* ``items`` y ``mitems``, que proporciona elementos inmutables y mutables respectivamente.
* ``pairs`` y ``mpairs`` que proporcionan el elemento y un número de índice (inmutable y mutable respectivamente)

.. code-block:: nim

  for index, item in ["a", "b"].pairs:
    echo item, " at index ", index
    # => a at index 0
    # => b at index 1

**Los ámbitos y la declaración de block**

Las declaraciones de flujo de control tienen una característica aún no cubierta:
Abren un nuevo ámbito (contexto).
Esto significa que en el siguiente ejemplo, ``x`` no es accesible fuera del bucle:

.. code-block:: nim

  while false:
    var x = "hi"
  echo x  # does not work

Una sentencia while (para) introduce un bloque implícito.
Los identificadores solo son visibles dentro del bloque que han sido declarados.
La instrucción de ``block`` se puede usar para abrir un nuevo bloque explícitamente:

.. code-block:: nim

  block myblock:
    var x = "hi"
  echo x # does not work either

La etiqueta del bloque (``myblock`` en el ejemplo) es opcional.

**break Declaración**

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

**continue declaración**

Al igual que en muchos otros lenguajes de programación,
una instrucción de ``continue`` comienza la siguiente iteración inmediatamente:

.. code-block:: nim

  while true:
    let x = readLine(stdin)
    if x == "": continue
    echo x

**when declaración**

Ejemplo:

.. code-block:: nim

  when hostOS == "windows":
    echo "running on Windows!"
  elif hostOS == "linux":
    echo "running on Linux!"
  elif hostOS == "macosx":
    echo "running on Mac OS X!"
  else:
    echo "unknown operating system"

La instrucción ``when`` es casi idéntica a la instrucción ``if``, pero con estas diferencias:

* Cada condición debe ser una expresión constante ya que es evaluada por el compilador.
* Las declaraciones dentro de una rama no abren un nuevo alcance.
* El compilador comprueba la semántica y produce código solo para las
declaraciones que pertenecen a la primera condición que se evalúa como ``true``.

La instrucción ``when`` es útil para escribir código específico de plataforma,
similar a la construcción ``#ifdef`` en el lenguaje de programación C.


Declaraciones y sangría
-----------------------

Ahora que cubrimos las declaraciones de flujo de control básico,
volvamos a las reglas de sangría de Nim.

En Nim hay una distinción entre declaraciones simples y declaraciones complejas .
Las declaraciones simples no pueden contener otras declaraciones: La asignación,
las llamadas a procedimientos o la declaración de devolución pertenecen a las declaraciones simples.

Las declaraciones complejas como ``if``, ``when`` , ``for`` , ``while``
pueden contener otras declaraciones.
Para evitar ambiguedades, las declaraciones complejas siempre deben estar sangradas,
pero las declaraciones simples y simples no:

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

Como regla general, se permite la sangría dentro de las expresiones después de los operadores,
un paréntesis abierto y después de las comas.

Con paréntesis y punto y coma ``( ; )`` puede usar sentencias donde solo se permite una expresión:

.. code-block:: nim

  # computes fac(4) at compile time:
  const fac4 = (var x = 1 ; for i in 1..4: x *= i ; x)


Procedimientos
--------------

Para definir nuevos comandos como echo y readLine en los ejemplos,
se necesita el concepto de un ``procedimiento``.
(Algunos idiomas los llaman métodos o funciones ).
En Nim, los nuevos procedimientos se definen con la palabra clave ``proc``:

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

Este ejemplo muestra un procedimiento llamado sí que hace una pregunta al usuario y
devuelve verdadero si contestó "sí" (o algo similar) y
devuelve falso si respondió "no" (o algo similar).
Una declaración de retorno abandona el procedimiento (y, por lo tanto, el bucle while) inmediatamente.
La ( sintaxis : cadena):
``bool`` describe que el procedimiento espera un parámetro llamado pregunta de tipo
``cadena`` y devuelve un valor de tipo ``bool`` .
El tipo bool está integrado:
los únicos valores válidos para ``bool`` son ``true`` y ``false``.
Las condiciones en las sentencias if o while deben ser de tipo ``bool``.

Alguna terminología:
En la pregunta de ejemplo se llama un parámetro (formal),
"Debería ..." se llama un argumento que se pasa a este parámetro.

**Variable de resultado**

Un procedimiento que devuelve un valor tiene una variable de ``result``
implícita declarada que representa el valor de retorno.
Una declaración de ``return`` sin expresión es una abreviatura para el ``return result``.
El valor del ``result`` siempre se devuelve automáticamente al final de un procedimiento
si no hay una declaración de ``return`` en la salida.

.. code-block:: nim

  proc sumTillNegative(x: varargs[int]): int =
    for i in x:
      if i < 0:
        return
      result = result + i

  echo sumTillNegative() # echos 0
  echo sumTillNegative(3, 4, 5) # echos 12
  echo sumTillNegative(3, 4 , -1 , 6) # echos 7

La variable de ``result`` ya está declarada implícitamente al inicio de la función,
por lo que declararla de nuevo con ``var result``, por ejemplo,
la sombrearía con una variable normal del mismo nombre.
La variable de resultado también ya está inicializada con el valor predeterminado del tipo.
Tenga en cuenta que los tipos de datos referenciales serán ``nil`` al inicio del procedimiento y,
por lo tanto, pueden requerir una inicialización manual.


**Parámetros**

Los parámetros son inmutables en el cuerpo del procedimiento.
De forma predeterminada, su valor no se puede cambiar porque esto permite al
compilador implementar el paso de parámetros de la manera más eficiente.
Si se necesita una variable mutable dentro del procedimiento,
debe declararse con ``var`` en el cuerpo del procedimiento.
Es posible sombrear el nombre del parámetro, y en realidad un idioma:

.. code-block:: nim

  proc printSeq(s: seq, nprinted: int = -1) =
    var nprinted = if nprinted == -1: s.len else: min(nprinted, s.len)
    for i in 0 .. <nprinted:
      echo s[i]

Si el procedimiento necesita modificar el argumento para la persona que llama,
se puede usar un parámetro ``var``:

.. code-block:: nim

  proc divmod(a, b: int; res, remainder: var int) =
    res = a div b        # integer division
    remainder = a mod b  # integer modulo operation

  var x, y: int
  divmod(8, 5, x, y) # modifies x and y
  echo x
  echo y


En el ejemplo, ``res`` y ``remainder`` son ``var parameters``.
Los parámetros de la var pueden ser modificados por el procedimiento y
los cambios son visibles para la persona que llama.
Tenga en cuenta que el ejemplo anterior sería mejor utilizar una tupla como
valor de retorno en lugar de usar los parámetros var.

**Declaración de descarte**

Para llamar a un procedimiento que devuelve un valor solo por sus efectos
secundarios e ignorando su valor de retorno, se debe usar una declaración de ``discard``.
Nim no permite tirar silenciosamente un valor de retorno:

.. code-block:: nim

  discard yes("May I ask a pointless question?")

El valor de retorno se puede ignorar implícitamente si el ``proc`` / ``iterator``
llamado se ha declarado con el pragma ``discardable``:

.. code-block:: nim

  proc p(x, y: int): int {.discardable.} =
    return x + y

  p(3, 4) # now valid


**Argumentos con nombre**

A menudo, un procedimiento tiene muchos parámetros y
no está claro en qué orden aparecen los parámetros.
Esto es especialmente cierto para los procedimientos que construyen un tipo de datos complejo.
Por lo tanto, los argumentos de un procedimiento se pueden nombrar,
de modo que quede claro qué argumento pertenece a qué parámetro:

.. code-block:: nim

  proc createWindow(x, y, width, height: int; title: string;
                    show: bool): Window =
    ...

  var w = createWindow(show = true, title = "My Application",
                       x = 0, y = 0, height = 600, width = 800)


Ahora que usamos argumentos con nombre para llamar a ``createWindow``,
el orden de los argumentos ya no importa.
También es posible mezclar argumentos nombrados con argumentos ordenados, pero no es muy legible:

.. code-block:: nim

  var w = createWindow(0, 0, title = "My Application",
                       height = 600, width = 800, true)


El compilador comprueba que cada parámetro recibe exactamente un argumento.

**Valores predeterminados**

Para que el proceso ``createWindow`` sea ​​más fácil de usar, debe proporcionar ``default values`` ;
Estos son valores que se utilizan como argumentos si el llamante no los especifica:

.. code-block:: nim

  proc createWindow(x = 0, y = 0, width = 500, height = 700,
                    title = "unknown",
                    show = true): Window =
    ...

  var w = createWindow(title = "My Application", height = 600, width = 800)

Ahora la llamada a ``createWindow`` solo necesita establecer los valores que difieren de los valores predeterminados.

Tenga en cuenta que la inferencia de tipos funciona para parámetros con valores predeterminados;
No hay necesidad de escribir ``title: string = "unknown"``, por ejemplo.

**Procedimientos sobrecargados**

Nim proporciona la capacidad de sobrecargar procedimientos similares a C++

.. code-block:: nim

  proc toString(x: int ):  string  =  ...
  proc toString(x: bool ): string  =
    if x: result = "true"
    else: result = "false"

  echo toString(13)   # llama al toString (x: int) proc
  echo toString(true) # llama al proceso toString (x: bool) proc


(Tenga en cuenta que ``toString`` suele ser el operador $ en Nim).
El compilador elige el proceso más apropiado para las llamadas a ``toString`` .
Aquí no se explica cómo funciona exactamente este algoritmo de resolución de sobrecarga
(se especificará en el manual en breve).
Sin embargo, no conduce a sorpresas desagradables y se basa en un algoritmo de unificación bastante simple.
Las llamadas ambiguas se reportan como errores.

Los operadores

La biblioteca Nim hace un uso intensivo de la sobrecarga,
una de las razones es que cada operador como + es solo un proceso sobrecargado.
El analizador le permite usar operadores en ``infix notation (a + b)`` o ``prefix notation (+ a)``.
Un operador de infijo siempre recibe dos argumentos, un operador de prefijo siempre uno.
(Los operadores de Postfix no son posibles, porque esto sería ambiguo:
``a @ @ b significa (a) @ (@b)`` o ``( a @ ) @ (b)`` ?
Siempre significa ``(a) @ (@b)`` , porque no hay operadores de postfix en Nim.)

Aparte de unos cuantos incorporado operadores de palabras clave tales como ``and``, ``or``, ``not``,
los operadores siempre constan de los siguientes caracteres:
``+ - * \ / <> = @ $ ~ &%! ? ^. |``

Se permiten operadores definidos por el usuario.
Nada le impide definir su propio operador ``@!? + ~``,
Pero hacerlo puede reducir la legibilidad.

La precedencia del operador está determinada por su primer carácter.
Los detalles se pueden encontrar en el manual.

Para definir un nuevo operador, encierre el operador en backticks:

.. code-block:: nim

  proc `$` (x: myDataType): string = ...
    # now the $ operator also works with myDataType, overloading resolution
    # ensures that $ works for built-in types just like before

La notación backticks también se puede usar para llamar a un operador como cualquier otro procedimiento:

.. code-block:: nim

  if `==`( `+`(3, 4), 7): echo "true"


**Forward Declarations**

Cada variable, procedimiento, etc. debe ser declarado antes de que pueda ser utilizado.
(La razón de esto es que no es trivial evitar esta necesidad en un lenguaje que
admita la programación meta tan ampliamente como lo hace Nim).
 Sin embargo, esto no se puede hacer para procedimientos recursivos mutuos:

.. code-block:: nim

  # forward declaration:
  proc even(n: int): bool
  proc odd(n: int): bool

.. code-block:: nim

  proc odd(n: int): bool =
    assert(n >= 0) # makes sure we don't run into negative recursion
    if n == 0: false
    else:
      n == 1 or even(n-1)

  proc even(n: int): bool =
    assert(n >= 0) # makes sure we don't run into negative recursion
    if n == 1: false
    else:
      n == 0 or odd(n-1)


Aquí lo ``odd`` depende de ``even`` y viceversa.
Por lo tanto, ``even`` es necesario introducirlo en el compilador antes de que esté completamente definido.
La sintaxis para tal declaración de reenvío es simple:
Simplemente omita el ``=`` y el cuerpo del procedimiento.
El ``assert`` solo agrega condiciones de borde y se cubrirá más adelante en la sección Módulos .

Las versiones posteriores del lenguaje debilitarán los requisitos para las declaraciones a plazo.

El ejemplo también muestra que el cuerpo de un proceso puede consistir en una
sola expresión cuyo valor se devuelve implícitamente.


Iteradores
----------

Volvamos al ejemplo de conteo simple:

.. code-block:: nim

  echo "Counting to ten: "
  for i in countup(1, 10):
    echo i


Se puede escribir un proceso de conteo que admita este bucle? Intentemos:

.. code-block:: nim

  proc countup(a, b: int): int =
    var res = a
    while res <= b:
      return res
      inc res

Sin embargo, esto no funciona.
El problema es que el procedimiento no solo debe regresar,
sino que debe regresar y continuar después de que una iteración haya finalizado.
Este retorno y continuar se llama una declaración de rendimiento.
Ahora lo único que queda por hacer es reemplazar la palabra clave proc por iterador y aquí está:

Nuestro primer iterador:

.. code-block:: nim

  iterator countup(a, b: int): int =
    var res = a
    while res <= b:
      yield res
      inc res

Los iteradores son muy similares a los procedimientos, pero hay varias diferencias importantes:

* Los iteradores solo pueden ser llamados desde los bucles.
* Los iteradores no pueden contener una declaración de ``return`` (y procs no pueden contener una declaración de ``yield`` ).
* Los iteradores no tienen una variable de ``result`` implícita .
* Los iteradores no son compatibles con la recursión.
* Los iteradores no se pueden declarar hacia delante, porque el compilador debe poder alinear un iterador.
(Esta restricción desaparecerá en una versión futura del compilador).

Sin embargo, también puede usar un iterador de ``closure`` para obtener un conjunto diferente de restricciones.
Ver iteradores de primera clase para más detalles.
Los iteradores pueden tener el mismo nombre y parámetros que un proc,
ya que esencialmente tienen sus propios espacios de nombres.
Por lo tanto, es una práctica común ajustar los iteradores en procesos del mismo
nombre que acumulan el resultado del iterador y lo devuelven como una secuencia,
como ``split`` del módulo strutils .

Tipos basicos
-------------

Esta sección trata los tipos básicos integrados y las operaciones que están disponibles para ellos en detalle.

Bool
----

El tipo booleano de Nim se llama ``bool`` y consiste en los dos valores predefinidos ``true`` y ``false`` .
Las condiciones en ``while``, ``if``, ``elif`` y ``when`` deben ser de tipo ``bool``.

Los operadores ``not, and, or, xor, <, <=,>,> =,! =, ==`` están definidos para el tipo ``bool``.
El ``and`` y ``or`` los operadores de realizar la evaluación de cortocircuito. Por ejemplo:

.. code-block:: nim

  while p != nil and p.name != "xyz":
    # p.name is not evaluated if p == nil
    p = p.next

Char
----

El tipo caracter es llamado ``char``. Su tamanio es siempre 1 byte,
no puede representar caracteres UTF-8;
Pero puede representar uno de los bytes que conforman un caracter multi-byte UTF-8.
La razon es eficiencia: Para la mayoria de casos de uso mas comunes,
los programas resultantes manejan UTF-8 adecuadamente por que UTF-8 fue
especificamente disaniado para esto.
El literal de un ``char`` es encerrado en comilla simple comun.

Los caracteres pueden sen comparados con ``==``, ``<``, ``<=``, ``>``, ``>=``.
El operador ``$`` convertira un ``char`` a ``string``.
Caracteres  no se pueden mezclar con enteros;
Para obtener el valor ordinal de un ``char`` usar el proc ``ord``.
Convertir desde entero a ``char`` se hace con el proc ``chr``.

String
------

Las variables de cadena son **mutables**, por lo que es posible agregarlas a una cadena, y es bastante eficiente.
Las cadenas en Nim son Cero-terminadas (null terminator) y
tienen un campo de longitud la longitud de una cadena se puede recuperar con el
proc ``len`` incorporado; la longitud nunca cuenta el cero final (terminator).
Acceder a la terminación cero es un error,
solo existe para que una cadena Nim pueda convertirse a un ``cstring`` sin hacer una copia.

El operador de asignación para cadenas copia la cadena.
Puedes usar el ``&`` operador para concatenar cadenas y ``add`` para agregar a una cadena.

Las cadenas se comparan utilizando su orden lexicográfico.
Todos los operadores de comparación son compatibles.
Por convención, todas las cadenas están codificadas en UTF-8.
Por ejemplo, al leer cadenas de archivos binarios, son simplemente una secuencia de bytes.
La operación de índice ``s[i]`` significa el i-th ``char`` de ``s``, no el i-th ``unichar``.

Una variable de cadena se inicializa con la cadena vacía ``""``.

Integers
--------

Nim tiene estos tipos de enteros incorporados:
``int int8 int16 int32 int64 uint uint8 uint16 uint32 uint64``.

El tipo de entero predeterminado es ``int``.
Los literales enteros pueden tener un *sufijo de tipo*
para especificar un tipo de entero no predeterminado:

.. code-block:: nim

  let
    x = 0     # x is of type ``int``
    y = 0'i8  # y is of type ``int8``
    z = 0'i64 # z is of type ``int64``
    u = 0'u   # u is of type ``uint``


La mayoría de los enteros se utilizan para contar objetos que residen en la memoria,
por lo que ``int`` tiene el mismo tamaño que un puntero.

Los operadores comunes ``+ - * div mod <<= ==! =>> =`` se definen para
enteros Los operadores ``and / or xor not`` también se definen para enteros,
y proporcionan operaciones *bitwise*.
El desplazamiento de bits a la izquierda se realiza con el ``shl``,
a la derecha cambiando con el operador ``shr``.
Los operadores de cambio de bits siempre tratan sus argumentos como *sin firmar* (Unsigned).

Las operaciones Unsigned son todas del tipo wrap around;
No pueden resultar en errores de overflow o underflow.

Floats
------

Nim tiene estos tipos de punto flotante incorporados: ``float float32 float64``.

El tipo float predeterminado es ``float``.
En la implementación actual, ``float`` es siempre de 64 Bits.

Los literales flotantes pueden tener un *sufijo de tipo*
para especificar un tipo de float no predeterminado:

.. code-block:: nim

  var
    x = 0.0      # x is of type ``float``
    y = 0.0'f32  # y is of type ``float32``
    z = 0.0'f64  # z is of type ``float64``

Los operadores comunes ``+ - * /  <  <=  ==  !=  >  >=``
estan definidos para float y siguen los Standards de IEEE-754.

La conversión automática de tipos en expresiones con diferentes tipos de flotación:
El tipo más pequeño se convierte en el tipo más grande.

Los tipos Enteros **no** se convierten a tipos de punto flotante automáticamente,
ni viceversa.

Type Conversion
---------------

La conversión entre tipos numéricos se realiza utilizando el tipo como una función:

.. code-block:: nim

  var
    x: int32 = 1.int32     # same as calling int32(1)
    y: int8  = int8('a')   # 'a' == 97'i8
    z: float = 2.5         # int(2.5) rounds down to 2

Enumerations
------------

A una variable de un tipo de enumeración solo se le puede asignar uno de los valores especificados de la enumeración.
Estos valores son un conjunto de símbolos ordenados.
Cada símbolo es mapeado a un valor entero internamente.
El primer símbolo está representado en tiempo de ejecución por 0, el segundo por 1 y así sucesivamente. Por ejemplo:

.. code-block:: nim

  type Direction = enum
    north, east, south, west

  var x = south     # `x` is of type `Direction`; its value is `south`
  echo x            # writes "south"

Todos los operadores de comparación se pueden utilizar con tipos de enumeración.

El símbolo de una enumeración se puede calificar para evitar ambigüedades:
``Direction.south``.

El operador ``$`` puede convertir cualquier valor de enumeración a su nombre,
y el ``ord`` puede convertirlo a su valor entero subyacente.

Para interactuar mejor con otros lenguajes de programación,
a los símbolos de enumeración se les puede asignar un valor ordinal explícito.
Sin embargo, los valores ordinales debe estar en orden ascendente.

Ordinal types
-------------

Enumeraciones, enteros, ``char`` y ``bool`` (y subranges) se llaman tipos ordinales.
Los tipos ordinales tienen algunas operaciones especiales:

-----------------     --------------------------------------------------------
Operacion             Comentario
-----------------     --------------------------------------------------------
``ord(x)``            Retorna el valor entero usado para representar `x`
``inc(x)``            Incrementa `x` por 1
``inc(x, n)``         Incrementa `x` por `n`; `n` es un entero
``dec(x)``            Decrementa `x` por 1
``dec(x, n)``         Decrementa `x` por `n`; `n` es un entero
``succ(x)``           Retorna el sucesor de `x`
``succ(x, n)``        Retorna el `n` sucesor de `x`
``pred(x)``           Retorna el predecesor de `x`
``pred(x, n)``        Retorna el `n` predecesor de `x`
-----------------     --------------------------------------------------------

Subranges
---------

Un tipo de subrango es un rango de valores de un entero o tipo de enumeración (el tipo base). Ejemplo:

.. code-block:: nim

  type MySubrange = range[0..5]

``MySubrange`` es un Subrango de ``int`` que solo puede contener los valores desde ``0`` a ``5``.
Asignar cualquier otro valor a una variable de tipo ``MySubrange`` es un
Error en tiempo de compilación o en tiempo de ejecución.

Arrays
------

Un Array es un contenedor de longitud fija simple.
Cada elemento en un Array tiene el mismo tipo.
El tipo de índice de la matriz puede ser cualquier tipo ordinal.

Los Array se pueden construir usando ``[]``:

.. code-block:: nim

  type IntArray = array[0..5, int]   # Array indexed con 0..5

  var x: IntArray
  x = [1, 2, 3, 4, 5, 6]

  for i in low(x)..high(x):
    echo x[i]

La notación ``x [i]`` se usa para acceder al elemento i-th de ``x``.
El acceso al Array siempre se comprueba con límites
(en tiempo de compilación o en tiempo de ejecución).
Estos chequeos pueden ser deshabilitados a través de pragmas o
invocando el compilador con ``--boundChecks:off`` en la línea de comando.

El operador de asignacion copia todo el contenido del Array.

Sequences
---------

Las secuencias son similares al Array pero de longitud dinámica,
que pueden cambiar durante el tiempo de ejecución (como las cadenas).

Las secuencias siempre se indexan con un ``int`` que comienza en la posición ``0``.

Las secuencias pueden ser construidas por el constructor de Array ``[]``
en conjunción con el operador ``@``, es decir ``@[]``.

Ejemplo:

.. code-block:: nim

  var
    x: seq[int]
    x = @[1, 2, 3, 4, 5, 6] # @ turns the array into a sequence allocated on the heap

Las variables de secuencia se inicializan con ``@[]``

Open arrays
-----------

**Nota**: Openarrays solo se puede utilizar para parámetros.

A menudo, los arreglos de tamaño fijo resultan ser demasiado inflexibles;
Los procedimientos deben ser capaz de manejar Array de diferentes tamaños.
El `openarray` permite esto.
Los Openarrays siempre se indexan con un ``int`` que comienza en la posición 0.
Cualquier Array con un tipo de base compatible se puede pasar a un parámetro openarray.

.. code-block:: nim

  var
    fruits:   seq[string]       # reference to a sequence of strings that is initialized with '@[]'
    capitals: array[3, string]  # array of strings with a fixed size

  capitals = ["New York", "London", "Berlin"]   # array 'capitals' allows assignment of only three elements
  fruits.add("Banana")          # sequence 'fruits' is dynamically expandable during runtime
  fruits.add("Mango")

  proc openArraySize(oa: openArray[string]): int =
    oa.len

  assert openArraySize(fruits) == 2     # procedure accepts a sequence as parameter
  assert openArraySize(capitals) == 3   # but also an array type

El tipo openarray no puede ser anidado:
Los openarrays multidimensionales no son soportados porque esto rara vez es necesario
y no se puede hacer de manera eficiente.

Varargs
-------

Un parámetro ``varargs`` es como un parámetro openarray.
Sin embargo es también un medio para pasar un número variable de argumentos a un procedimiento.
El compilador convierte la lista de argumentos a un Array automáticamente:

.. code-block:: nim

  proc myWriteln(f: File, a: varargs[string]) =
    for s in items(a):
      write(f, s)
    write(f, "\n")

  myWriteln(stdout, "abc", "def", "xyz")
  # is transformed by the compiler to:
  myWriteln(stdout, ["abc", "def", "xyz"])

Esta transformación solo se realiza si el parámetro varargs es el
ultimo parámetro en el encabezado del procedimiento.
También es posible realizar conversiones de tipo en este contexto:

.. code-block:: nim

  proc myWriteln(f: File, a: varargs[string, `$`]) =
    for s in items(a):
      write(f, s)
    write(f, "\n")

  myWriteln(stdout, 123, "abc", 4.0)
  # is transformed by the compiler to:
  myWriteln(stdout, [$123, $"abc", $4.0])

En este ejemplo ``$`` se aplica a cualquier argumento que se pase al parámetro ``a``.

Slices
------

Los segmentos se parecen a los tipos de subranges en la sintaxis,
pero se utilizan en una diferente contexto.
Una división es solo un objeto de tipo División que contiene dos límites, ``a`` y ``b``.
Por sí mismo, un Slice no es muy útil, pero otros tipos de colección
definen operadores que aceptan Slice para definir rangos.

.. code-block:: nim

  var
    a = "Nim is a progamming language"
    b = "Slices are useless."

  echo a[7..12] # --> 'a prog'
  b[11..^2] = "useful"
  echo b # --> 'Slices are useful.'


Objects
-------

El tipo predeterminado para empaquetar diferentes valores juntos en una sola
estructura con un nombre es el tipo Objeto.
Un objeto es un Value Type, lo que significa que cuando un objeto se asigna a
una nueva variable todo sus componentes también se copian.

Cada tipo de objeto ``Foo`` tiene un constructor ``Foo(campo: valor)``,
donde todos sus campos pueden ser inicializados.
Los campos no especificados obtienen su valor por defecto.

.. code-block:: nim
  type
    Person = object
      name: string
      age: int

  var person1 = Person(name: "Peter", age: 30)

  echo person1.name # "Peter"
  echo person1.age  # 30

  var person2 = person1 # copy of person 1

  person2.age += 14

  echo person1.age # 30
  echo person2.age # 44


  # the order may be changed
  let person3 = Person(age: 12, name: "Quentin")

  # not every member needs to be specified
  let person4 = Person(age: 3)
  # unspecified members will be initialized with their default
  # values. In this case it is the empty string.
  doAssert person4.name == ""

Los campos de objeto que deben ser visibles desde fuera del módulo de definición
tienen que estar marcados con ``*``.

.. code-block:: nim

  type
    Person* = object # the type is visible from other modules
      name*: string  # the field of this type is visible from other modules
      age*: int


Tuples
------

Las tuplas se parecen mucho a lo que has visto de los objetos.
Son tipos de valor donde el operador de asignación copia cada componente.
Sin embargo, a diferencia de los tipos de objetos, los tipos de tuplas están escritos estructuralmente,
lo que significa que diferentes tipos de tuplas son *equivalentes*
si especifican campos de el mismo tipo y del mismo nombre en el mismo orden.

El constructor ``()`` puede usarse para construir tuplas.
El orden de los campos en el constructor deben coincidir con el orden en la tupla.
Pero a diferencia de los objetos, un nombre para el tipo de tupla no puede ser utilizado aquí.

Al igual que el tipo de objeto, la notación ``t.field`` se usa para acceder a un campo de la tupla.
Otra notación que no está disponible para objetos es ``t[i]``
para acceder al campo ``i``. Aquí ``i`` debe ser un entero.

.. code-block:: nim

  type
    # type representing a person:
    # A person consists of a name and an age.
    Person = tuple
      name: string
      age: int

    # Alternative syntax for an equivalent type.
    PersonX = tuple[name: string, age: int]

    # anonymous field syntax
    PersonY = (string, int)

  var
    person: Person
    personX: PersonX
    personY: PersonY

  person = (name: "Peter", age: 30)
  # Person and PersonX are equivalent
  personX = person

  # Create a tuple with anonymous fields:
  personY = ("Peter", 30)

  # A tuple with anonymous fields is compatible with a tuple that has
  # field names.
  person = personY
  personY = person

  # Usually used for short tuple initialization syntax
  person = ("Peter", 30)

  echo person.name # "Peter"
  echo person.age  # 30

  echo person[0] # "Peter"
  echo person[1] # 30

  # You don't need to declare tuples in a separate type section.
  var building: tuple[street: string, number: int]
  building = ("Rue del Percebe", 13)
  echo building.street

  # The following line does not compile, they are different tuples!
  #person = building
  # --> Error: type mismatch: got (tuple[street: string, number: int])
  #     but expected 'Person'


Aunque no es necesario declarar un tipo para que una tupla lo use,
las tuplas creadas con diferentes nombres de campo serán considerados objetos diferentes
a pesar de tener los mismos tipos de campo.

Los campos de tuplas son siempre públicos, no necesitan ser explícitamente marcados para ser exportados,
a diferencia de por ejemplo, los campos en un tipo de objeto.


Modulos
=======

Nim admite la división de un programa en partes con un concepto de módulo.
Cada módulo está en su propio archivo.
Un módulo puede acceder a los símbolos de otro módulo utilizando la instrucción ``import``.
Sólo los símbolos de nivel superior que están marcados con un asterisco ``*`` se exportan.

.. code-block:: nim
  # Modulo A
  var
    x*, y: int

  proc `*`*(a, b: seq[int]): seq[int] =
    # allocate a new sequence:
    newSeq(result, len(a))
    # multiply two int sequences:
    for i in 0..len(a)-1: result[i] = a[i] * b[i]

  when isMainModule:
    # test the new ``*`` operator for sequences:
    assert(@[1, 2, 3] * @[1, 2, 3] == @[1, 4, 9])

El módulo anterior exporta ``x`` y ``*``, pero no ``y``.

Las instrucciones de nivel superior de un módulo se ejecutan al inicio del programa.
Esto se puede usar para inicializar estructuras de datos complejas, por ejemplo.

Cada módulo tiene una constante mágica especial ``isMainModule``,
que es verdadera si el módulo se compila como el archivo principal.
Esto es muy útil para incrustar pruebas dentro de el módulo como se muestra en el ejemplo anterior.

Excluding symbols
-----------------

La declaración normal ``import`` traerá todos los símbolos exportados.
Estos pueden estar limitados por nombres de símbolos que deberían excluirse con
el calificador ``except``:

.. code-block:: nim

  import mymodule except y


From statement
--------------

Ya hemos visto la simple declaración ``import`` que solo importa todos símbolos exportados.
Una alternativa que solo importa los símbolos listados es ``from import``:

.. code-block:: nim

  from mymodule import x, y, z

La declaración ``from`` también puede forzar la calificación del espacio de nombres en símbolos,
por lo que los símbolos están disponibles, pero necesitan ser calificados para ser utilizados.

.. code-block:: nim

  from mymodule import x, y, z

  x()           # use x without any qualification

.. code-block:: nim

  from mymodule import nil

  mymodule.x()  # must qualify x with the module name as prefix

  x()           # using x here without qualification is a compile error

Dado que los nombres de los módulos son generalmente largos para ser descriptivos,
también puede definir un alias más corto para usar cuando califiques símbolos.

.. code-block:: nim

  from mymodule as m import nil
  m.x()         # m is aliasing mymodule


Include statement
-----------------

La declaración ``include`` hace algo fundamentalmente diferente a importar un módulo:
Simplemente incluye el contenido de un archivo.
``include`` es útil para dividir un módulo grande en varios archivos:

.. code-block:: nim

  include fileA, fileB, fileC


**FIN**

* Entonces, hasta aqui hemos terminado con lo básico.
