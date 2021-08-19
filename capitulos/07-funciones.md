# Funciones

Las funciones asociadas a un objeto se llaman *métodos*, y tiene acceso al mismo mediante `this`.

Se pueden asignar funciones a variables. El identificador de la función es en sí una variable que contiene esa función.

Las funciones pueden anidarse, o declararse dentro de un bloque de código. Las funciones anidadas tienen acceso a las variables de la función que las contiene.

```js
function nombre(parms) {
    /* body */
}
```

Si indicamos `return` sin valor, o no incluimos `return`, la función retorna ***undefined***.

## Expresión declaración de función

La declaración de una función puede formar parte de una expresión, o formar parte de un argumento en una invocación de otra función:

```js
let f = function foo() { /* body */ };
```

Se puede definir e invocar al mismo tiempo:

```js
let tensquared = function(x) { return x * x; }(10);
```

Cuando forma parte de una expresión, el nombre de la función es opcional, ya que de hecho aunque indiquemos nombre, no se crea tal variable. Solo será necesario indicarlo para funciones recursivas.

Cuando se declaran las funciones mediante la sintaxis normal, las funciones pueden utilizarse antes de su declaración (*hoisting*). Sin embargo, si se usa la declaracón dentro de una expresión, esto no se aplica.

## Arrow functions

Se trata de una sintaxis muy compacta:

```js
const sum = (a, b) => { return a + b; }
```

Si el cuerpo de la función es un solo `return`, todavía se puede hacer más compacto:

```js
const sum = (a, b) => a + b;
```

Además, si la lista de parámetros solo contiene uno, se pueden omitir los paréntesis. Si no hay parámetros hay que incluir el par de paréntesis.

## Invocación

Para invocar una función, se hace normalmente: `foo(parms)`.

Para definir un método a partir de una función ***f***:

```js
o.m = f;
```

Tiene acceso a `this`. Y para invocarlo: `o.m(parms)`. También se puede utilizar la sintaxis de corchete (`o["m"](parms)`).

La sentencia `new` causa la invocación del constructor.

Por otro lado, las funciones son objetos en sí, con lo que tienen a su vez métodos. En concreto, los métodos `call()` y `apply()` invocan la función. Además, permiten definir el valor de `this`, con lo que cualquier función se puede ejecutar como si fuese un método de cualquier objeto (aunque no lo sea).

Las funciones pueden ser invocadas implícitamente, como por ejemplo los *getters* y *setters*, los métodos `toString()` o `valueOf()` en una conversión implícita, etc.

## Argumentos y parámetros

Cuando en la invocación se proporcionen menos argumentos que los parámetros declarados, el resto se establece en ***undefinded***. Si se proporcionan más, se ignoran los excesivos.

Es posible dar valor por defecto a los parámetros; no tienen por qué ser los últimos definidos.

A la hora de tomar los valores de los argumentos en la llamada, se pueden especificar los nombres de parámetro:

```js
function suma(primero, segundo=10) { return primero + segundo; }
suma(5, 7);  // retorna 12 (5+7)
suma(5);  // retorna 15 (5+10)
suma(segundo=20, primero=5);  // retorna 25 (5+20)
suma(segundo=20);  // retorna 30 (20+10, se ignora 'segundo=')
```

Si el último parámetro es `...rest`, recogerá el resto de argumentos que excedan la lista de parámetros de la función. Entonces, el parámetro ***rest*** en la función es un *array* con esos valores.

A la hora de invocar una función que requiere varios parámetros, podemos usar el operador `...` que es una forma de *desempaquetar* los valores de un *array*: `foo(...array)`.

También es posible definir *arrays* como parámetros, y quedan fácilmente desempaquetados:

```js
function foo(n, [a,b,c], [d,e]) { /*...*/ }
a1 = [1, 2, 3];
a2 = [10, 20];
foo(3, a1, a2);
```

También puede hacerse con objetos (mediante llaves en lugar de corchetes).

## Propiedades de funciones

Dado que las funciones son en sí objetos, se pueden añadir propiedades en la función, de tal modo que dichas propiedades actuarán de forma similar a variables estáticas.
