# Clases

En *JavaScript* la herencia es a través de prototipos, como se ha visto. Si dos objetos heredan del mismo protoptipo, se dice que son de la misma clase.

Al construir un objeto mediante `new`, se utiliza (invoca) al constructor de la clase especificada para inicializar el objeto. Hay que tener en cuenta algo importante: la propiedad `prototype` del constructor será el objeto prototipo que heredará el objeto al crearse.

Para crear una clase, simplemente hay que definir una función (que es el constructor en sí). Esta función inicializa los valores del objeto mediante `this`. El objeto prototipo se define dando valor a la propiedad `prototype` del constructor.

```js
function MiClase(parm) {    // el constructor
    this.prop = parm;
}

let punto = {x: 10, y: 15};
MiClase.prototype = punto;    // acabamos de definir el constructor

let o = new MiClase(5);
o.prop;    // 5
o.x;    // 10
o.y;    // 15
```

El operador `instanceof` lo que comprueba específicamente es si ***o*** hereda el objeto ***MiClase.prototype***.

```js
o instanceof MiClase;    // true
```

## class

Una sintaxis más moderna es utilizar `class`.

```js
class MiClase {
    constructor(parm) {    // el constructor
    this.prop = parm;
    }
    doble(n) {return 2*n;}    // método: va a MiClase.prototype
    punto = {x: 10, y: 15};    // propiedad: no va al prototipo
}
```

## Métodos estáticos

Se hace prefijando `static` ante el nombre del método al declararlo. En ese caso, el método no va al protitipo, sino a la misma clase.
