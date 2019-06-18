Tutorial de Nim 

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
La palabra clave ``elif``es la abreviatura de ``else`` ``if`` , y es útil para evitar una sangría excesiva. 
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

