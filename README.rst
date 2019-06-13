Tutorial de Nim (Parte I)
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

Literales de cuerdas y personajes
--------------
* Los literales de cadena están encerrados entre comillas dobles; Literales de caracteres en comillas simples. Los caracteres especiales se escapan con \ : \ n significa nueva línea, \ t significa tabulador, etc. También hay literales de cadena en bruto 

.. code-block:: nim
    r"C:\program files\nim""


* En literales crudos, la barra invertida no es un personaje de escape.

La tercera y última forma de escribir literales de cadena son literales de cadena larga . Están escritos con tres citas: "" "..." "" ; pueden abarcar varias líneas y el \ no es un carácter de escape tampoco. Son muy útiles para incrustar plantillas de código HTML, por ejemplo.


Comentarios
--------------
Los comentarios comienzan en cualquier lugar fuera de una cadena o literal de caracteres con el # de carácter de hash . Los comentarios de documentación comienzan con ## :

.. code-block:: nim
 # A comment.
 var myVariable: int ## a documentation comment

Los comentarios de documentación son tokens; solo se permiten en ciertos lugares en el archivo de entrada ya que pertenecen al árbol de sintaxis! Esta característica permite generadores de documentación más simples.

Los comentarios de varias líneas se inician con # [ y terminan con ] # . Los comentarios multilínea también pueden ser anidados.

.. code-block:: nim
 #[
 You can have any Nim code text commented
 out inside this with no indentation restrictions.
      yes("May I ask a pointless question?")
  #[
     Note: these can be nested!!
  ]#
 ]#

Números
--------------

La sentencia **var**
--------------

La declaración de asignación
--------------

Constantes
--------------

La declaración de *let*
--------------

Declaraciones de flujo de control
--------------