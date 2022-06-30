# Tipos, valores y variables

*JavaScript* dispone de **tipos primitivos** (números, *strings*, booleanos), que incluyen ***null*** y ***undefined***, que son valores tales que cada uno es de su propio tipo específico. También existe el tipo *Symbol*.

A parte de los tipos primitivos, existe el tipo **objeto**, que es una colección de valores no ordenados.

El tipo objeto es mutable, y los tipos primitivos son inmutables.

Por otro lado, un *array* es una colección de valores ordenados. Las funciones y las clases son también de un tipo especial. La biblioteca estándar define otros tipos.

La gestión de objetos en memoria pasa por un *garbage collector*.

Los tipos son convertidos automáticamente según el contexto. No hay tipado estricto. Las variables y constantes no tienen tipo.

## Números

*JavaScript* representa los números mediante punto flotante de 64 bits. Algunas operaciones usan los números como enteros de 32 bits para calcular el resultado.

### Literales numéricos

Cualquier literal puede llevar, opcionalmente, el signo delante.

Los literales enteros pueden representarse:

- Secuencia de dígitos decimales (***0-9***) que no empieza por cero (excepto el cero mismo). Representa un entero en base 10.
- Prefijo ***0x*** (o ***0X***) seguido por una secuencia de dígitos hexadecimales (***0-F***, *case-insensitive*). Representa un entero en base 16.
- Prefijo ***0b*** (o ***0B***) seguido por una secuencia de dígitos binarios (***0-1***). Representa un entero en base 2.
- Prefijo ***0o*** (o ***0O***) seguido por una secuencia de dígitos octales (***0-7***). Representa un entero en base 8.

Si a un literal entero **decimal** se le añade un punto decimal (***.***) y una parte fraccionaria **decimal**, se obtiene un literal de punto flotante. En este caso, si se omite la parte entera, se considerará esta cero. Si se omite la parte fraccionaria, se considerará un literal entero.

A cualquier literal numérico se le puede añadir una parte exponencial, consistente en un carácter ***e*** (o ***E***), un signo opcional, y un exponente entero decimal.

Para aumentar la legibilidad de los literales numéricos, se pueden intercalar guiones bajos (***\_***).

### Operaciones matemáticas

Cuando una operación produce un valor mayor al representable, el resultado es un valor especial: la constante ***Infinity***. Para operaciones con resultado indefinido, retorna la constante ***NaN***. Este último se compara distinto a cualquier valor, incluido a sí mismo.

No se deberían comparar dos números en punto flotante por igualdad. Por eso existe un nuevo tipo, *BigInt*: enteros de 64 bits. Un literal de este tipo debe llevar el sufijo ***n***. Para convertir a *BigInt* se usa la función `BigInt()`. No se pueden mezclar número regulares con números *BigInt* en una expresión (ninguno de estos tipos es más general que el otro). Sin embargo sí se pueden comparar entre ellos.

El objeto ***Math*** proporciona operaciones matemáticas (no acepta *BigInt*).

## Fecha y hora

Para la fecha y hora existe el objeto ***Date***. Se convierten a número dando un *timestamp*.

## Texto

Un *string* es una secuencia ordenada inmutable de valores *Unicode* de 16 bits (no existe el tipo carácter). La indexación de sus elementos (igual que pasa con los *arrays*) es *0-based*. Usa la codificación *UTF-16*. Los caracteres del *basic multilingual plane* se representan con un solo valor, pero podemos encontrar *strings* con más elementos que caracteres (si hay *surrogate pairs*).

Los literales *string* aceptan comillas simples (***'***), dobles (***"***) y *backticks* (***`***). Cualquier tipo acepta los otros tipos en su interior. Se pueden definir *strings* en varias líneas terminando cada línea con una barra invertida (***\\***), excepto la última.

```js
s = "Esto es un \
string en varias líneas."
```

Para definir un salto de línea se usará ***\\n***. Adicionalmente, un literal definido con *backticks* puede ser multilínea, de tal modo que los saltos de línea tecleados forman parte del *string*.

Dado que en *HTML* se pueden definir también los *strings* tanto con comillas simples como dobles, se pueden combinar para que no haya interferencias:

```html
<button onclick="alert('Thank you')">Click Me</button>
```

### Secuencias de escape

Para incluir en el *string* apariciones del carácter comilla delimitadora, hay que incicar *escaped* (prefijado de ***\\***) el carácter comilla que estemos usando (***\\"***, ***\\'***, o ***\\`***).

Disponemos de ***\\0*** (carácter nulo), ***\\b*** (retroceso), ***\\t*** (tabulador horizontal), ***\\n*** (salto de línea), ***\\v*** (tabulador vertical), ***\\f*** (*form feed*), ***\\r*** (retorno de carro), ***\\\\*** (*backslash*).

***\\xnn*** sirve para especificar un carácter *Unicode* con dos caracteres hexadecimales. ***\\unnnn*** hace lo mismo con 4 dígitos hexa, y ***\\u\{nn...n}*** hace lo propio con una cantidad arbitraria de estos dígitos.

Delante de cualquier otro carácter, la barra invertida es ignorada.

### Operaciones con *strings*

Para concatenar se usa el operador `+`. Para saber la longitud de un *string*, se usa su propiedad `length`. Un *string* tiene numerosas utilidades, pero ninguna cambia el contenido (*strings* son inmutables); en todo caso retornan un nuevo *string*. Sus elementos son accesibles mediante corchetes y un número de índice (***[n]***).

### Literales plantilla

Los *strings* definidos con *backticks* (*template literals*) aceptan expresiones *JavaScript* encerradas entre `${` y `}`. A parte, los saltos de línea tecleados pasan a formar parte del contenido.

Además, si se prefija el nombrede una función a este tipo de literales, se invoca esa función pasándole como parámetro el literal. Por ejemplo, la función `String.raw()` retorna el *string* sin procesar los *escapes*:

```js
`\n`.length;  // retorna 1
String.raw(`\n`).length;  // retorna 2
```

Se pueden ver los *backticks* como paréntesis de la llamada.

## Booleanos

Toman los valores ***true*** o ***false***.

Los valores ***undefined***, ***null***, cero (positivo o negativo), ***NaN*** y *string* vacío se convierten a ***false***. Todo lo demás se evalúa a ***true***.

## null y undefined

***null*** es el único valor de su clase, e indica un valor nulo. Por otro lado, ***undefined*** es el valor que tienen las varibales antes de ser definidas, o lo que retorna una referencia a una propiedad o elemento de un *array* inexistentes, o una función sin retorno explícito de valor, o parámetros no definidos. También es el único valor de su tipo. Mientras que ***null*** es un objeto, ***undefined*** es una constante predefinida.

La expresión `undefined == null` es cierta, pero `undefined === null` (valor y tipo) es falsa.

## Símbolos

Los símbolos sirven para acceder a propiedades de objetos. Normalmente se accede a ellos mediante *strings*. Sin embargo, se puede acceder a estos mediante un símbolo: un código de identificación único que permite acceder a estos elementos. Se crean mediante la función `Symbol()`. Esta función nunca retorna el mismo valor, ya que siempre retorna un ID único. Un símbolo tiene el método `toString()`, que retorna el *string* ***Symbol()***. Si se invoca pasándole un *string* (opcional), ese *string* será retornado por `toString()`: retornará ***Symbol(\<st>)***, donde 'st' es ese *string*.

La función `Symbol.for()` es similar, con la diferencia de que el símbolo retornado depende del argumento que se le pasa. En este sentido, la expresión `Symbol.for("texto") == Symbol.for("texto")` retorna ***true***.

## El objeto global

Las propiedades del objeto global incluyen: constantes globales (como ***undefined***), funciones globales (como `parseInt()`), constructores (como `Date()`), objetos globales (como ***Math***).

En un navegador web, el objeto global es ***window***, en *Node* es ***global***, etc. En todo caso, se puede acceder, desde cualquier entorno, mediante ***globalThis***.

## Tipos mutables e inmutables

Los valores primitivos (***undefined***, ***null***, booleanos, números y *strings*) son inmutables. Los objetos (incluyendo *arrays* y funciones) son mutables.

Los tipos primitivos se comparan por **valor**. En los objetos se comparan las referencias.

```js
let a = "3", b = 3;  // String y número
a == b;  // true: mismo valor
a === b;  // false: mismo valor, pero tipo distinto

let c = "txt", d = "txt";  // Dos strings iguales
c == d;  // true: mismo valor
c === d;  // true: mismo valor, mismo tipo

let o = {x: 1}, p = {x: 1};  // Dos objetos, mismas propiedades
o == p;  // false: son referencias a dos objetos separados (aunque iguales)
o === p; // false: idem (aunque sean del mismo tipo)

let q = [], r = []; // Dos arrays vacíos
q == r;  // false: son referencias a dos arrays separados (aunque iguales)
q === r;  // false: idem (aunque sean del mismo tipo)
t = q;
t == q;  // true: t y q son referencias al mismo array
t === q;  // true
```

Si queremos crear una copia de un *array*, hay que crearlo y hacer la copia explícitamente (elemento a elemento). Para comparar dos *arrays* por valor, hay que comparar sus elementos.

## Conversiones

*JavaScript* convierte tipos sobre la marcha, según contexto. Las conversiones explícitas se pueden hacer con las funciones globales `Boolean()`, `Number()` y `String()`. Los valores suelen tener, además, un método `toString()` (normalmente equivalente a la global `String()`). En los valores numéricos, este método aceptan un argumento opcional que indica la base (entre 2 y 36).

También en los valores numéricos, existen los métodos `toFixed()` (retorna un *string* con el número de posiciones decimales especificadas), `toExponential()` (notación exponencial, con 1 dígito antes del punto decimal y el número de posiciones decimales especificadas) y `toPrecision()` (se especifica el número de dígitos significativos, notación puede ser exponencial).

Para conversión a números también existen las funciones `parseInt()` y `parseFloat()`.

**Todos** los **objetos** evalúan a ***true***. Los objetos pueden definir su propio método `toString()` (conversión a *string*) y/o `valueOf()` (retorna la instancia en sí misma).

## Declaración y asignación

Las variables se declaran con `let`, y las constantes con `const`. Se pueden declarar varias variables (o constantes) en la misma sentencia, separadas por comas. Las variables pueden inicializarse o no. Las constantes se deben inicializar todas.

### Scope

#### let y const

El ámbito de la variable (o constante) es la región del código fuente donde se define esta (*block scope*), desde el punto donde se declara hasta el final del bloque. Si está declarada en la misma sentencia `for`, su *scope* es el cuerpo entero de dicho `for`.

Variables y constantes definidas fuera de todo bloque, tienen ámbito global. Es accesible en el archivo fuente donde está definida, a partir del punto donde se declara.

No se puede declarar dos veces la misma variable (o constante) en el mismo ámbito, aunque sí puede declararse (*override*) en un *scope* interior.

#### var

También se pueden declarar variables (no constantes) mediante `var` (usado, sobre todo para código *legacy*).

Las variables declaradas así dentro de función no tienen *block scope* sino *function scope*, independientemente de si se declaran al principio de la función, o dentro de un bloque interior a esta. Este *scope* se extiende en **toda la función**, tanto antes (*hoisting*) como después de la declaración, e independientemente del nivel de anidamiento de la declaración (antes de la declaración tienen valor ***undefined***, aunque la declaración las inicialice).

Si la declaración está fuera de la función, es una variable global, pero se comporta como una propiedad del objeto global.

Las variables se pueden redeclarar con `var` más de una vez (no con `let`).

En el modo *strict* no se pueden usar variables o constantes que no estén inicializadas.

### Destructuring assignment

```js
let [x,y] = [1,2];  // let x=1, y=2
[x,y] = [x+1,y+1];  // x=x+1, y=y+1
[x,y] = [y,x];  // Permite hacer swap de las variables en una sola asignación
let [x,y] = [1];  // x=1, y undefined
[x,y] = [1,2,3];  // x=1, y=2
[,x,,y] = [1,2,3,4];  // x=2, y=4
```

Útil también para recoger valor de retorno de una función que retorne un *array*.

Se puede indicar 'resto de valores' mediante puntos suspensivos:

```js
[x, ...y] = [1, 2, 3, 4];  // x=1, y=[2,3,4]
```

*Arrays* anidados:

```js
let [a, [b, c]] = [1, [2,2.5], 3];  // a=1, b=2, c=2.5
```

En la parte derecha de la asignación no es obligatorio que haya un *array*; cualquier iterable vale:

```js
let [first, ...rest] = "Hello";  // first="H", rest=["e","l","l","o"]
```

También se puede usar un objeto para hacerlo:

```js
let color = {r: 0.0, g: 0.0, b: 0.0, a: 1.0};  // un objeto
let {r, g, b} = color;  // r=0.0, g=0.0, b=0.0
const {sin, cos, tan} = Math;  // sin=Math.sin, cos=Math.cos, tan=Math.tan (funciones)
const {cos: cosine, tan: tangent} = Math;  // cosine=Math.cos, tangent=Math.tan;
```

Se puede complicar cuando hay *arrays* y objetos anidados.
