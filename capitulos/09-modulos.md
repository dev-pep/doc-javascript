# Módulos

En *JavaScript* cada archivo de código fuente es un módulo. Todas las constantes, variables, funciones, y clases declaradas en él son privadas del módulo, a no ser que se exporten (con `export`). En un módulo, estos elementos no tienen ámbito global, sino que su ámbito es el módulo.

El código en un módulo se ejecuta en modo estricto. Fuera de un módulo, para tener modo estricto hay que indicar el *string* `use strict`, al principio de todo de un *script*, o como primera sentencia de una función:

```js
function foo() {
    'use strict';
    /* resto */
}
```

En modo estricto hay una serie de cosas que no están permitidas mientras que sí lo están en modo normal, como el uso de variables no declaradas, la eliminación de variables y objetos con `delete`, etc.

Para exportar un elemento, se debe usar `export`, **en el top level** del archivo:

```js
export const PI = Math.PI;
export function degreesToRadians(d) { return d * PI / 180; }
export class Circle {
    constructor(r) { this.r = r; }
    area() { return PI * this.r * this.r; }
}
```

Se puede hacer lo mismo sin indicar `export`, y después (al final del archivo, por ejemplo) incluir:

```js
export { Circle, degreesToRadians, PI };
```

Si se hace así, se pueden renombrar los elemtnos al exportar:

```js
export { Circle as circ, PI, degreesToRadians as deg };
```

Para importar la exportación de estos elementos, desde otro archivo de código, se utiliza `import`. Este se suele colocar al principio del archivo, pero no es necesario, porque los elementos importados son *hoisted* (disponibles en cualquier punto del archivo que importa).

El módulo puede indicar (como máximo) un `export default`. En este caso se puede tratar de un elemento que no tenga nombre (literal objeto, etc.). Este tipo de exportaciones son fáciles de importar.

Para importar una exportación *default*:

```js
import miNombre from './modulo.js';
```

Al ser una importación *default*, le podemos dar cualquier nombre local.

Las importaciones solo pueden aparecer en el nivel superior de un archivo (no en clases, funciones o bloques). El nombre del módulo a usar puede ir entre comillas simples o dobles, y puede indicar una ruta relativa o absoluta, pero siempre empezando por un directorio (en rutas relativas, empezará por ***./*** o ***../***).

Para importar varios elementos:

```js
import { Circle, degreesToRadians } from "./modulo.js";
```

Para importar todos los elementos ***no default***:

```js
import * as ob from "./modulo.js";
```

En este caso, todos los elementos no default del módulo pasarán a ser propiedades del objeto ***ob***.

Para importar tanto elementos no *default* como el elemento *default*:

```js
import miNombre, {unaVar, unaFun} from "./modulo.js";
```

También es posible importar un módulo que no tenga exportaciones:

```js
import "./modulo_sinexp.js"
```

En este caso, el módulo **se ejecuta** la primera vez que es importado.

A veces es últil renombrar las importaciones:

```js
import { Circle as circ, PI, degreesToRadians as deg } from "./modulo.js";
```
