# Práctica 8

## Ejercicio 1
¿Explique claramente a qué se denomina excepción?

Condición inesperada o inusual, que surge durante la ejecución del programa y no puede ser manejada en el contexto local.

## Ejercicio 2
¿Que debería proveer un lenguaje para el manejo de las excepciones? ¿Todos los lenguajes
lo proveen?

Para manejar excepciones, los lenguajes deben proveer:

* Una forma de definirlas
* Una forma de lanzarlas/alcanzarlas/invocarlas
* Una forma de manejarlas
* Un criterio de continuación

## Ejercicio 3
¿Qué ocurre cuando un lenguaje no provee manejo de excepciones? ¿Se podría simular?
Explique como lo haría.

Se podría simular el manejo de excepciones mediante:

1. el chequeo manual de las condiciones que generarían la excepción, y
2. la invocación, también manual, de la rutina para manejar dicha excepción (esto sería el manejador).

## Ejercicio 4
Cuando se termina de manejar la excepción, la acción que se toma luego es importante.
Indique

	a) ¿Qué modelos diferentes existen en este aspecto?
	b) Dé ejemplos de lenguajes que utilicen cada uno de los modelos presentados anteriormente.
	c) ¿Cuál de esos modelos es más inseguro y por qué?


**a**-**b**. Los modelos con respecto al criterio de continuación luego del manejo de la excepción son:

* Terminación: se corta la ejecución de la unidad que provocó la excepción y se entrega el control al manejador.
	* a nivel de bloque: el bloque en ejecución corta su ejecución. Ej.: Ada.
	* a nivel de sentencia: la sentencia en ejecución corta su ejecución. Ej.: CLU.
* Reasunción: luego de manejar la excepción, se continúa desde donde el manejador fue invocado. Ej.: PL1.

La diferencia entre Reasunción y Terminación a nivel de sentencia radica en que en reasunción __siempre__ se invoca al manejador en la sentencia que lanza la excepción, permitiendo que se reanude la ejecución en la sentencia posterior, mientras que en terminación a nivel de sentencia __no se asegura__ que la invocación al manejo de la excepción se produzca en el mismo ámbito en el que la misma fue lanzada (por ejemplo, si se propaga estáticamente sobre las sentencias contenedoras, luego no volvería a ejecutar desde luego de la sentencia que provocó la excepción).*

**c**. ¿Inseguro desde qué perspectiva?

## Ejercicio 5
La propagación de los errores, cuando no se encuentra ningún manejador asociado, no se
implementa igual en todos los lenguajes. Realice la comparación entre CLU, PL/1, ADA, respecto a este
tema. Defina la forma en que se implementa en un lenguaje conocido por Ud.

* CLU
	* Al producirse una excepción, se termina el procedimiento donde se levantó la excepción y devuelve el control al llamante inmediato donde se debe encontrar el manejador, si el manejador se encuentra en ese ámbito, se ejecuta y luego se pasa el control a la sentencia siguiente a la que está ligado dicho manejador, en caso contrario, si el manejador no se encuentra en ese lugar, la excepción se propaga estáticamente en las sentencias asociadas, esto significa que el proceso se repite para las sentencias incluidas estáticamente.
	
        En caso de no encontrar ningún manejador en el procedimiento que hizo la llamada se levanta una excepción failure y devuelve el control, terminando todo el programa.
	
        
* PL/I
	* En PL/I no se realiza propagación de errores cuando no se encuentra un manejador asociado debido a que los manejadores se ligan dinámicamente con las excepciones, es decir, una excepción siempre estará ligada con el último manejador definido.
	
        
* ADA
	* Al producirse una excepción, se termina la unidad, bloque, paquete o tarea dónde se alcanza la unidad, en caso de que el manejador se encuentra en ese ámbito, se ejecuta, si no la excepción se propaga dinámicamente, esto significa que se vuelve a levantar en otro ámbito.

        En Ada siempre tener en cuenta el alcance, puede convertirse en anónima!
        
* C++
	* Al producirse una excepcion, se termina la unidad o rutina, en caso de que la excepción se encuentre se propaga la excepción. Si una excepción es repetidamente propagada y no machea con ningún manejador, entonces una función terminate() es ejecutada automáticamente. En el caso de no haber esperado dicha excepción no se propaga la misma y la función especial unexpected() se ejecuta auntomaticamente, la cual generalmente ejecuta abort(), que provoca el final del programa. Unexpected() puede ser redefinida por el programador.
	
## Ejercicio 7
* a) Cambio el proceso Manejador1 por la instrucción _ON CONDITION Manejador1 begin x := x + 1; end_ y así.
* b) ADA cuenta con el bloque _declare_ que es una rutina dentro del proceso. Conviene declarar el raise dentro de ese bloque para no matar la ejecución del proceso.

## Ejercicio 9
Indique diferencias y similitudes entre Phyton y Java con respecto al manejo de excepciones.
    
Ambos hacen uso del try finally y pueden recibir una lista de posibles excepciones, python tiene la facilidad de tener la clausura 'else' que manejaria cualquier excepción la cual no haya sido tratada previamente, mientras que en java hay que especificar dicha excepción.

## Ejercicio 10
¿Qué modelo de excepciones implementa Ruby?. Qué instrucciones específicas provee el lenguaje para manejo de excepciones y cómo se comportan cada una de ellas?
    
Ruby utiliza el modelo terminal para el manejo de sus excepciones, este hace una utilización similar del try catch de java, pero estos se llaman raise rescue, la clausura raise levanta una excepción, si dicha excepción es la que espera la clausura rescue, esta la maneja, hay una 'else' al igual que en python que sirve para manejar cualquier excepción que no haya sido contemplada.

## Ejercicio 11
~~Dado que el manejo de excepciones de ADA es por terminación a nivel de bloque, entonces el lanzamiento de una excepción en la definición de un _if else_ termina con la ejecución de este bloque, y pasa la excepción al manejador mas cercano. En este punto x == 11 y se lanza una expceción anónima. Como no hay manejador en el bloque manejador de _e_, entonces propaga al del proceso. Tampoco hay manejador en el bloque del proceso y propaga afuera del mismo, es decir, al ambito del procedure main. En este ambito hay un manejador  de excepciones y siendo una anónima, entra al _when other_, donde x pasa a ser x == 13. Luego, x queda con 13 al finalizar la ejecución del programa.~~

No pasa a ser anónima dado que el alcance de la excepción en ADA estará determinado por el alcance de la variable donde fué declarada. Siendo que _e: exception_ está declarado en el main, entonces tiene alcance global, por lo tanto, al ser propagada en el raise del proceso _UNO_, el manejador del main sabe cual es la excepcion y ejecuta el _when e_. Sin embargo, suponiendo que se ejecuta el proceso _DOS_, este proceso redefine _e_, por lo tanto, cuando se propaga el raise, entra en el manejador de _when other_ del proceso main.

## Ejercicio 12
Tenemos el lanzamiento de la excepción _error1_ en la definición del proceso _UNO_. Al ser levantada esta excepción, termina la ejecución del procedimiento, ignorando la existencia de manejadores dentro del mismo. Posteriormente, pasa a ser ejecutado el manejajor existente en la linea siguiente a la llamada del proceso: 

	exception when error1 -> 
		x := x+1; 
		y := y + 1; 
	end;
	
