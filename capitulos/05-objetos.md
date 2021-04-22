# Objetos

Los objetos son colecciones no ordenadas de propiedades, las cuales tienen un nombre y un valor. El nombre es un *string* (aunque también puede ser un símbolo). Se podría ver como un *map* de *string* (o símbolo) a valor. Las propiedades pueden añadirse y borrarse.

Los *strings*, números y booleanos, a pesar de no ser objetos, se pueden comportar como objetos inmutables.

Los objetos se pueden crear mediante literales objeto, con `new`, o mediante `Object.create()`. Para acceder a los miembros se hace con sintaxi de punto (`ob.prop`) o de corchetes (`ob["prop"]`), aunque si la propiedad contiene espacios se debe usar la última (`ob["una propiedad"]`).

Mediante `new`, se invoca al constructor:

```js
let o = new Object();  // Equivale a {}
let a = new Array();  // Equivale a []
let d = new Date();  // Objeto fecha
let r = new Map();  // Mapping (conjunto key/value)
```

Las propiedades de un objeto tienen, a parte de su valor, una serie de atributos de propiedad: *writable* (se puede cambiar su valor), *enumerable* (la propiedad es retornada en un bucle `for/in`) y *configurable* (se pueden cambiar estos atributos, y la propiedad puede ser eliminada). Los tres están activados por defecto.

## Herencia

La herencia en *JavaScript* funciona algo diferente que en otros lenguajes: al crearse un objeto, este hereda automáticamente un conjunto de propiedades de un "objeto prototipo" (a parte de lo correspondiente a su clase). Si no se especifica objeto prototipo, se hereda automáticamente el objeto `Object.prototype`. Las propiedades de un objeto que no provienen de ese objeto prototipo se llaman *own properties* (propiedades propias).

Un objeto creado a partir de ***Object*** hereda un objeto ***Object.prototype***, si es de ***Array***, hereda ***Array.prototype***, etc. No todos los objetos tienen propiedad ***prototype***, pero sí tienen todos un objeto prototipo. Esta propiedad define lo que heredarán los objetos que se basen en él.

Al crear un objeto mediante `Object.create()`, pasamos como argumento el objeto prototipo directamente. Si le pasamos ***null***, el objeto no heredará absolutamente nada, ni siquiera métodos útiles como `toString()`. Si se quiere crear un objeto ordinario, se puede hacer `Object.create(Object.prototype)`.

Si intentamos acceder a una propiedad de un objeto y dicha propiedad no se encuentra entre las propiedades propias del objeto, se buscará en el objeto prototipo que ha heredado el objeto. Si este objeto prototipo tampoco tiene dicha propiedad en sí mismo, pero a su vez tiene un objeto prototipo que ha heredado, se buscará en ese prototipo del prototipo. Y así sucesivamente hasta encontar esa propiedad en la cadena de prototipos, o hasta llegar al final.

## Propiedades

Si la propiedad no se encuentra, retornará ***undefined***. Si se intenta acceder a una propiedad de algo que evalúe a ***null*** o ***undefined***, se produce error.

Cuando se usa sintaxis de corchetes (***o["prop"]***), lo que hay en el interior de los corchetes debe ser convertible a *string* o *symbol*.

Cuando hacemos una asignación a una propiedad, si esta no existe, la añade al objeto. Se puede ver un objeto como un *associative array*.

Si un objeto tiene una propiedad heredada (no propia), al asignar valor a la misma, se crea tal propiedad (propia) en el objeto, de modo que enmascara la propiedad del objeto prototipo. Pero si hereda una propiedad *readonly*, no está permitido hacer esa asignación.

A la hora de eliminar propiedades, el comando `delete` solo elimina propiedades propias, no heredadas.

Para saber si un objeto tiene una propiedad propia concreta, se usa el operador `in`, el método `hasOwnProperty()`, o el método `propertyIsEnumerable()`. Cualquiera de estos métodos precisa un *string* o símbolo. También se puede simplemente acceder a la propiedad.

## Enumeración de propiedades

Mediante `for/in` se itera por todas las propiedades enumerables del objeto (propias y heredadas).

Para iterar las distintas propiedades del objeto, a parte de hacerlo con `for/in` se puede usar:
- `Object.keys()` retorna un *array* con los nombres (*strings*) de las propiedades **enumerables** del objeto, pero solo las propias, no heredadas.
- `Object.getOwnPropertyNames()` además retorna los nombres de las propiedades propias que no son enumerables.
- `Object.getOwnPropertySymbols()` igual que la anterior, pero para propiedades propias que son símbolos.
- `Reflect.ownKeys()` combina las dos anteriores.

El orden de enumeración de las funciones anteriores (y otras como `JSON.stringify()`) es el siguiente:
- Propiedades *string* cuyos nombres son números no negativos, en orden numérico.
- Resto de propiedades *string* en el orden que fueron añadidas.
- Propiedades *symbol* en el orden que fueron añadidas.
El orden de `for/in` no está tan definido, aunque se suele hacer de la forma definida para las propiedades propias, y lo mismo para las propiedades heredadas, e ir siguiendo la cadena de prototipos. Las propiedades enmascaradas no se enumeran.

## Copia de objetos

Es posible copiar un objeto mediante la función `Object.assign()`. Esta recibe un primer parámatro con el objeto destino, donde se copiarán todas las propiedades **propias** de los subsiguientes objetos especificados en los argumentos posteriores.

```js
Object.assign(odest, osrc1, osrc2,...);  // añade las propiedades propias de los objetos fuente en el objeto destino
```

También se puede crear un objeto nuevo:

```js
o = Object.assign({}, osrc1, osrc2,...);
```

## Serialización de objetos

Para serializar un objeto se usa `JSON.Stringify()`, y para restaurarlo se usa `JSON.parse()`.

## Métodos de objeto

Los objetos suelen heredar el prototipo de `Object.prototype`. Esto les da algunas funcionalidades útiles, como `toString()` o `valueOf()` (conversión a otro tipo que no sea *string*).

Adicionalmente, si definimos el método `toJSON()`, será utilizado por `JSON.stringify()` para serializar el objeto.

## Sintaxis extendida de literales objeto

Disponemos de un *shorthand* de propiedades:

```js
// Si las variables x, y están definidas:
let o = {x, y};  // equivale a {x: x, y: y}
```

Para nombres de propiedades que no deseamos *hard code*, sino que se deciden en *run time*, el literal puede definirlas mediante una expresión entre corchetes:

```js
// Si la variable property_name y la función computed_name() están definidas:
let o = {
    prop_hardcoded: 25,
    [property_name]: 50,
    [computed_name()]: 100
}
```

Esto también se extiende a símbolos: una expresión entre corchetes puede retornar un símbolo o un *string*.

El operador *spread* (***...***) esparce, o desempaqueta las propiedades de un objeto (las propias, no las heredadas):

```js
let odest = {...osrc1, x: 50, ...osrc2};
```

En el ejemplo, se copian las propiedades de dos objetos, a parte de una propiedad que definimos explícitamente.

Si hay conflictos entre nombres de propiedades, la que prevalece es la última en ser definida.

Los métodos pueden definirse así en un literal:


```js
let square = {
  area: function() { return this.side ** 2; },
  side: 10
}
```
Hay un *shorthand*:

```js
let square = {
    area() { return this.side ** 2; },
    side: 10
}
```

El nombre del método puede contener espacios (habrá que usar comillas). Cuando se usa el *shorthand*, se pueden usar como nombre (o símbolo) cualquier expresión entre corchetes.

Se puede definir un método *getter* y/o un método *setter*. Se define añadiendo la palabra `get` o `set` antes del nombre de la función (o delante de la expresión entre corchetes). El nombre del *getter* o *setter* es el nombre de la propiedad que representan. Si se define solo un *setter*, actuará como una propiedad *write-only*.
