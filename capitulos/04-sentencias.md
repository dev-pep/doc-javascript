# Sentencias

Las sentencias están separadas entre sí por punto y coma (`;`).

Tipos de sentencias:

- Expresiones: al ejecutarse, se evalúa y retorna un valor. Puede conllevar efectos secundarios.
- Sentencias compuestas: bloque de sentencias entre llaves (`{}`).
- Sentencias vacías: un punto y coma extra.
- Condicionales, bucles y saltos.
- Declaraciones.
- Otros tipos.

## Condicionales

### if

```js
if(cond)
    sentencia1
else
    sentencia2
```

Se ejecuta la sentencia 1 si la condición es verdadera. De lo contrario, se ejecuta la 2. La cláusula `else` es opcional.

Pueden anidarse varios `if`. En este caso, si existen cláusulas `else` también y no está bien especificado (con llaves), puede no quedar claro a qué `if` corresponde cada `else`. La norma es que cada `else` ambiguo se corresponde al `if` más próximo posible.

Disponemos también de la cláusula `else if`.

```js
if(cond1)
    sentencia1
else if(cond2)
    sentencia2
else if(cond3)
    sentencia3
// ...
else  // opcional
    sentenciaN
```

### switch

```js
switch(expr) {
    case val1: /* código */
    case val2: /* código */
    /* ... */
    default: /* código*/
}
```

Para que la ejecución termine tras la cláusula `case` pertinente, habrá que añadir `break` al final de la misma.

La cláusula `default` es opcional.

## Bucles

### while

```js
while(cond)
    sentencia
```

### do/while

```js
do
    sentencia
while(cond)
```

### for

```js
for(init; test; iter)
    sentencia
```

### for/of

Se usa con objetos iterables. Los *arrays*, *strings*, *sets* y *maps* son iterables, por ejemplo.

```js
let data = [1, 2, 3, 4, 5, 6, 7, 8, 9], sum = 0;
for(let element of data)
    sum += element;
```

### for/in

Requiere un objeto, de cualquier tipo, aunque no sea iterable. La sintaxis en como la de `for/of`, pero en este caso va tomando el valor de los nombres de las propiedades que tiene el objeto.

## Saltos

`break` termina un bucle. `continue` itera. `return` retorna de una función. `yield` es como return, pero para retornar el siguiente valor en un generador. `throw` lanza una excepción.

Es posible etiquetar sentencias. Se usa para etiquetar sentencias asociadas a un bloque de código. De este modo, `break` y `continue` pueden usarse para algo distinto que el bucle actual. De hecho puede ser algo que no sea un bucle.

```js
etiq: if(cond)
{
    /* ... */
    break etiq;
}
```

### try / throw

```js
try {
    /* código a ejecutar */
}
catch(e1) {
    /* trata error e1 */
}
catch(e2) {
    /* trata error e2 */
}
/* otros catch */
finally {  //opcional
    /* se ejecuta siempre, pase lo que pase */
}

throw expr;  // expr es una expresión de cualquier tipo
```

## Declaraciones

Declaraciones de variables, constantes, funciones, clases.

### import y export

`import` se usa para importar elementos de otro archivo *JavaScript*, el cual debe exportar (`export`) estos.
