# Expresiones y operadores

Una expresión primaria es una expresión que no está formada por subexpresiones.

Un literal *array* es una expresión que genera un *array*. Es una lista de expresiones separadas por comas, y encerrada entre corchetes. Cualquiera de las expresiones puede ser a su vez un literal *array*.

```js
let matrix = [[1, 2, 3], [4, 5, 6], [7, 8, 9]];
```

Para incluir elementos indefinidos (***undefined***), se pueden escribir más de una coma seguida. Si la lista termina en una coma, esta es ignorada.

De forma análoga, un literal objeto es una lista de pares (propiedad: valor) entre llaves.

```js
let ob = {x: 33, y: 40};
```

Los valores, a su vez, pueden ser literales objeto.

Combinando y anidando literales *array* y objeto, tenemos el formato *JSON*.

Una expresión de definición de función retorna, de hecho, la función definida.

Las expresiones de acceso a miembros utilizan la sintaxis de punto (***.***) y de corchetes (***[]***):

```js
ob.x = 10;  // acceso a propiedad de objeto con punto
ob["y"] = 20;  // acceso a propiedad de objeto con corchetes
matrix[1][2] = 7;  // acceso a elemento del array, solo corchetes
```

Estos operadores (corchetes y punto) pueden combinarse. La evaluación se realiza de izquierda a derecha.

En *JavaScript* solo existen dos valores sin propiedades: ***undefined*** y ***null***. Por lo tanto, usar estos operadores de acceso en estos valores levanta error. Para evitarlo, se puede usar el acceso condicional: `?.` y `?.[]`:

```
a = ob?.prop;
b = arr?.[3];
```

En el caso que el objeto a la izquierda del operador de acceso a propiedades se evalúe a ***undifined*** o ***null***, la expresión se evalúa en ***null*** y no se realiza ningún intento de acceder a la propiedad.

Las expresiones de invocación de funciones y métodos se realizan mediante el nombre del método o función seguido de una lista de argumentos, separados por comas, entre paréntesis. El valor de la expresión es el retornado por `return`. También existe la invocación condicional, útil cuando el nombre de la función o método puede evaluarse a ***undifined*** o ***null***:

```
foo(1, 2);  // invocación normal
bar.?(1, 4, 4);  // invocación condicional
```

Para creación de objetos se usa `new`:

```js
new Object();
new Point(2, 3);
```

Si no se le pasan argumentos al constructor, se pueden omitir los paréntisis: `New Object;`. Esta expresion se evalúa al objeto creado.

## Operadores

### Operadores aritméticos

La mayoría de estos operadores admiten operandos *BigInt* y números regulares, siempre que no se mezclen ambos tipos.

La exponenciación (***\*****) tiene mayor precedencia que división (***/***), multiplicación (***\****) y módulo (***%***), que a su vez tienen mayor precedencia que suma (***+***) y resta (***-***).

Exponenciación se evalúa de derecha a izquierda. Se puede usar también `Math.pow()`. El módulo funciona también con números no enteros.

El operador ***+*** también permite concatenar *strings*. Cuando por lo menos uno de los operandos es *string* (o es un objeto que tiene conversión a *string*), se realiza la concatenación en lugar de la suma.

Los operadores ***+*** y ***-*** pueden ser unarios. Los operadores incrementales ***++*** y ***-*** pueden ser *prefix* o *postfix*.

Los operadores *bitwise* son: *and* (***&***), *or* (***|***), *xor* (***^***), *not* (***~***), desplazamiento izquierdo (***<<***), desplazamiento derecho con signo (***>>***) y desplazamiento derecho con *zero-fill* (***>>>***).

### Operadores relacionales

Son: igualdad (***==***), igualdad estricta (***===***), desigualdad (***!=***), desigualdad estricta (***!==***, contrario de ***===***) y los acostumbrados ***<***, ***>***, ***<=*** y ***>=***.

El operador ***in*** espera un operando izquierdo de tipo *string* o convertible a *string*. El operando derecho debe ser un objeto. Retornará ***true*** si el objeto contiene una propiedad con el nombre del *string*.

El operador `instanceof` espera un operando izquierdo de tipo objeto y un operando derecho que identifique una clase. Retorna ***true*** si el objeto pertenece a la clase (o a una derivada).

### Operadores lógicos

Son *and* (***&&***), *or* (***||***) y *not* (***!***). Los dos primeros cortocircuitan cuando es posible. El funcionamiento de ***||*** es curioso: si el primer operando tiene un valor verdadero, retorna directamente ese valor; de lo contrario, retorna el valor del segundo operando.

### Operadores de asignación

El operador ***=*** retorna el valor asignado, y se evalúa de derecha a izquierda (por ejemplo podemos escribir `a = b = 4;`).

También se admiten los operadores de asignación con operación, como ***+=***, ***-=***, ***=***, ***&=***, etc.

### Otros operadores

La función `eval()` retorna el valor que se la pasa como argumento, pero si se le pasa un *string*, lo parsea como código fuente *JavaScript* y retorna el valor de la expresión.

El operador `?:` es un operador ternario:

```js
exp1 ? exp2 : exp3;
```

Si la primera expresión evalúa a verdadero, retorna la segunda. En caso contrario, la tercera.

El operador ***??*** retorna el valor del primero de los operandos que esté definido:

```
let a = exp1 ?? exp2;
```

Si la primera expresión está definida, la retorna; de lo contrario, retorna la segunda.

El operador `typeof` espera un valor como operando, y retorna un *string* que indica el tipo de dicho valor.

El operador `delete` espera una propiedad de un objeto, que elimina del mismo.

El operador `void` evalúa su operando, descarta el valor retornado por este, y retorna `undefined`. Útil solo si el operando tiene *side effects*.

El operador coma (***,***) evalúa el primer operando, luego el segundo, y retorna el valor del segundo.
