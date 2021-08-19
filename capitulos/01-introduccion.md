# Introducción

Para enviar texto a la consola se usa la función `console.log()`. Podemos enviar un error a la consola mediante `console.error()` pasándole como argumento un valor, o un objeto o *array*, como por ejemplo el error ***err*** de una cláusula `catch()`:

```js
try() {
    /* código try */
}
catch(err) {
    /* ... */
    console.error(err);
    /* ... */
}
```

Cuando se trata de un *array* u objeto, se puede presentar de forma más estructurada con `console.table()`.

*JavaScript* es *case-sensitive*. El espacio en blanco está formado por caracteres espacio, tabulador, saltos de línea, etc., y sirve para separar *tokens*.

Acepta comentarios de línea (`//`) y multilínea (`/*` `*/`).

En cuanto a los identificadores, pueden empezar por una letra, guión bajo (***\_***) o signo dólar (***\$***). Los subsiguientes caracteres, además, pueden ser numéricos. *JavaScript* utiliza el juego de caracteres *Unicode*. Para identificadores y literales es posible utilizar caracteres *ASCII* para indicar un carácter *Unicode*, utilizando el prefijo `\u` seguido por 4 dígitos hexadecimales, o bien por uno a seis dígitos hexadecimales entre llaves.

Se utiliza el punto y coma (***;***) para separar sentencias. Si una línea contiene una sola sentencia, no es obligatorio. También se puede omitir al final del programa o de un bloque.

Las variables (y constantes) en *JavaScript* no tienen un tipo concreto asignado. Su valor cambia de tipo según el contexto.
