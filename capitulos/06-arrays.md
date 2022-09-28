# Arrays

Los *arrays* no tienen tipo, y el índice es numérico. Cada elemento puede ser de un tipo distinto, y de hecho se permite cualquier tipo (primitivo, objeto u *array*). Son *zero-based*. Los elementos no tienen por qué ir seguidos: puede haber huecos en la numeración. La propiedad `length` representa el número de elementos, si no hay huecos. Si los hay, es un número superior al índice máximo que tiene contenido.

Los *arrays* pueden construirse mediante un literal *array* (lista de valores entre corchetes, separados por comas), y puede contener como elementos otros *arrays* (*arrays* multidimensionales) u objetos. Para dejar huecos, repetir comas. También se puede usar el operador *spread* (***...***) sobre un objeto iterable. Con este operador se puede crear una *shallow copy* del *array*.

```js
let hexdigits = [..."0123456789ABCDEF"];  // array con 16 elementos "0", "1",...
```

Otra forma de crear un array es mediante el constructor `Array()`, al que se le pasa como argumentos los elementos iniciales (o nada para un *array* vacío). Si se le pasa un solo argumento, este indica el número de elementos del *array*.

Esto es similar a `Array.of()`, con la diferencia que en este último caso, al pasarle un solo argumento numérico, se creará un objeto con un solo elemento. Para crear un *array* a partir de cualquier objeto iterable:

```js
let ob = Array.from(iterable);
```

Si se asigna valor a un índice sin valor, crea ese elemento. Si el índice no es un entero no negativo, lo que hace es añadirle esa propiedad (como a un objeto). No existe el concepto de índice *out of bounds*.

Para añadir objetos, a parte de mediante asignación, se puede usar el método `push()`, que añade en la última posición. `pop()` elimina el último y lo retorna.

Para saber si un índice concreto está definido, se usa el operador `in`.

La propiedad `length` se puede escribir.

## Métodos de arrays

### Métodos que iteran sobre el array

Los métodos que iteran sobre los elementos del *array* no iteran en los huecos (si el *array* los tiene). Estos métodos toman un argumento que es una función, que será llamada con tres argumentos: el valor del elemento, en índice del mismo, y el *array* en sí. En ocasiones, el método acepta un segundo argumento, que actúa como si dicho método perteneciera al objeto pasado en este argumento, es decir, dentro del método, `this` es exactamente ese objeto. Estos métodos no cambian el *array*, aunque la función que pasamos como argumento sí puede hacerlo.

Para pasar una función, se puede hacer *inline* (*arrow syntax*), o indicando una función definida en otra parte. Esta función suele recibir tres argumentos (como se ha dicho), aunque si solo necesitamos el primero, por ejemplo, puede ser una función definida con un solo parámetro.

El método `forEach()` simplemente ejecuta la función indicada como argumento a cada elemento del *array*.

El método `map()` retorna un *array* cuyos valores son el valor devuelto por la función que le pasamos, sobre cada elemento del *array*.

El método `filter()` retorna un subconjunto del *array* original; la función pasada retornará ***true*** (o valor convertible a ***true***) en los elementos que estarán presentes en el *array* final.

Los métodos `find()` y `findIndex()` son como `filter()`, con la diferencia que el primero retorna el primer elemento verdadero, mientras que el segundo retorna el índice de ese elemento.

Del mismo modo, el método `every()` retorna ***true*** si la función es verdadera en todos los elementos del *array*; el método `some()` retorna ***true*** si la función es verdadera sobre algún elemento del mismo.

El método `reduce()` reduce el *array* a un solo valor, que va calculando al ir pasando por todos los elementos. El método tiene como primer parámetro la función iteradora, y como segundo, el valor inicial antes de las iteraciones. En cuanto a la función, tiene un número distinto de parámetros: el primer parámetro es el valor acumulado de las iteraciones, y los tres siguientes son los acostumbrados (valor, índice y *array*). El valor de retorno es el que se pasará como primer argumento a la función en la siguiente iteración. En la última iteración, el valor de retorno de la función será el resultado final. En la primera iteración, el valor de este primer argumento es el que le pasamos al método `reduce()` como valor inicial.

El método `reduceRight()` es como `reduce()`, pero iterando el *array* de final a principio.

### Otros métodos

El método `flat()` retorna un *array* similar al actual, pero con 1 (o más) niveles de anidamiento menos (en *arrays* multidimensionales). El parámetro indica el número de niveles a eliminar (por defecto 1).

```js
let o = [1, [2, [3, [4]]]];
o.flat(1);  // [1, 2, [3, [4]]], equivale a o.flat()
o.flat(2);  // [1, 2, 3, [4]]
o.flat(3);  // [1, 2, 3, 4]
o.flat(4);  // [1, 2, 3, 4]
```

El método `flatMap()` es como aplicar `map()` y luego allanar con `flat()`. Por ejemplo, `o.flatMap(foo)` equivale a `o.map(foo).flat()`.

El método `concat()` retorna un *array* con los elementos del *array* llamante, a los que se añaden todos los argumentos del método. Si alguno de los argumentos es un *array*, se concatenan sus elementos (no recursivamente) uno a uno.

Para trabajo con pilas, el método `push()` retorna la nueva longitud del *array*, mientras que `pop()` retorna el elemento. Métodos análogos, pero que trabajan al principio del *array* en lugar de al final, son `unshift()` y `shift()`. Estos cuatro métodos **sí modifican el *array***.

El método `slice()` retorna un *slice* del *array* (igual que *Python*, incluyendo índices negativos).

El método `splice()` elimina o inserta elementos en un *array*. El primer parámetro es el índice del primer elemento a eliminar. El segundo es la longitud (número de elementos) a eliminar (si se omite, se entiende hasta el final del *array*). El *array* inicial queda modificado, y el método retorna un *array* con los elementos eliminados. Los subsiguientes argumentos son elementos que serán insertados en el *array* en la posición indicada en el primer argumento. Si solo queremos insertar, el segundo argumento debe ser 0.

El método `fill()` rellena un *array* (o parte de él) con un valor determinado. Con un solo argumento, rellenará todo con ese valor. Con un segundo argumento, lo hará a partir del índice indicado en ese argumento. Con tres argumentos, el segundo y tercero representan un *slice*.

El método `copyWithin()` copia un *slice* de un *array* a otra posición del *array*. Nunca cambia la `length` del *array*. El primer parámetro indica el índice destino donde se copiará el primer elemento. El segundo elemento indica el índice del primer elemento a ser copiado (por defecto 0). El tercer elemento, junto con el segundo, especifican el *slice* de los elementos a ser copiados (por defecto hasta el final del *array*).

Los métodos `indexOf()` y `lastIndexOf()` buscan el primer (o último, respectivamente) elemento que sea igual (con ***===***) al argumento que se le pasa. Un segundo argumento indica el índice donde empezar la búsqueda (acepta números negativos). Si no lo encuentra, retorna ***-1***.

El método `includes()` retorna ***true*** si el *array* contiene un elemento igual que el especificado, o ***false*** en caso contrario.

El método `sort()` ordena el *array*, y retorna una referencia al mismo. Sin argumentos, ordena alfabéticamente y si el array contiene elementos indefinidos, los pasa al final. Por otro lado, se le puede pasar como argumento una función de comparación que toma dos valores como argumento y cuyo resultado es un número positivo si el primer argumento debe aparecer después del segundo, negativo en caso contrario, y cero si sus valores son equivalentes.

El método `reverse()` invierte el orden del *array* y retorna una referencia al mismo.

El método `join()` retorna un *string* con todos los elementos del *array* convertidos a *string* y concatenados. Se puede especificar *string* separador (por defecto ***\","***). Es el inverso del método `split()` de los *strings*.

El método `toString()` equivale al método `join()` sin argumentos.

Existe un método estático para comprobar si una variable es un *array*: `Array.isArray()`.

## Strings como arrays

Los *strings* se comportan como *arrays* de solo lectura de caracteres *Unicode UTF-16*.
