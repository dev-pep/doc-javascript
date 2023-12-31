# Clases

En *JavaScript* la herencia es a través de prototipos, como se ha visto. Si dos objetos heredan del mismo prototipo, se dice que son de la misma clase.

Al construir un objeto mediante `new`, se utiliza (invoca) al constructor de la clase especificada para inicializar el objeto. Hay que tener en cuenta algo importante: la propiedad `prototype` del constructor será el objeto prototipo que heredará el objeto al crearse.

## Factory function

Para crear una clase, se puede hacer a través de una *factory function*. Simplemente hay que definir una función (que será el constructor en sí). Esta función inicializa los valores del objeto mediante ***this***. Adicionalmente es posible definir, si así lo deseamos, el objeto prototipo de la clase, dando valor a la propiedad `prototype` de ese constructor:

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
        this.prop = parm;    // propiedad propia: no va al prototipo
    }
    doble(n) {return 2*n;}    // método: va a MiClase.prototype; no se usa keyword 'function'
    punto = {x: 10, y: 15};    // propiedad propia: tampoco va al prototipo; no se usa 'let'
    pelota = "verde";    // idem
}
```

## Herencia

```js
class MiDeriv extends MiClase {
    constructor(parm, otro) {    // el constructor
        super(parm);    // llamada al constructor padre
        this.otro = otro;  // propiedad propia: no va al prototipo
    }
    triple(n) {return 2*n;}    // método: va a MiDeriv.prototype; no se usa keyword 'function'
    punto = {x: 100, y: 150};    // propiedad propia; overrides el punto del padre
}
```

Si declaramos un objeto de tipo ***MiClase***, este tendrá las propiedades (propias) ***prop***, ***punto*** y ***pelota***. También dispone de un prototipo, que contendrá el constructor y el método ***doble()***. Este prototipo, contiene a su vez el prototipo de ***Object***.

Declarando un objeto de tipo ***MiDeriv***, este tendrá las propiedades (propias) ***prop***, ***otro***, ***punto*** (*overriden* con valores 100, 150) y ***pelota***. A parte, tendrá, en su prototipo, su constructor y el método ***triple()***. Este prototipo contiene también el prototipo recibido de la clase base, que contiene el constructor de la clase base y la función ***doble()***, a parte del prototipo de ***Object***.

Este mecanismo, a nivel práctico, obviando el tema del objeto prototipo (podemos obviarlo perfectamente), funciona como en otros lenguajes.

## Métodos estáticos

Se hace prefijando `static` ante el nombre del método al declararlo. En ese caso, el método no va al prototipo, sino a la misma clase.

## Getters, setters y propiedades privadas

Veamos un ejemplo de *getters* y *setters* (visto en capítulo sobre objetos):

```js
class Coche {
    constructor(marca, modelo, color) {
        this._marca = marca;
        this._modelo = modelo;
        this._color = color;
    }
    get marcaCoche() { return this._marca; }
    set marcaCoche(marca) { this._marca = marca; }
    set modeloCoche(modelo) { this._modelo = modelo; }  // write-only property
    get colorCoche() { return this._color; }  // read-only property
}
```

En este ejemplo, en lugar de acceder a las propiedades ***\_marca***, ***\_modelo*** y ***\_color***, accederemos a las "propiedades" ***marcaCoche***, ***modeloCoche*** y ***colorCoche***. Por convenio, las propiedades que no deben accederse directamente deberían empezar por guión bajo.

Sin embargo, las propiedades ***\_marca***, ***\_modelo*** y ***\_color*** siguen siendo accesibles.

Para que no lo sean, lo que se puede hacer es usando un método que tiende a desaparecer: construir la clase mediante una *factory function*, y en lugar de añadir esas propiedades, declararlas simplemente como variables locales del constructor. Existe un nuevo método de hacerlo: Últimamente existe la posibilidad de definir **propiedades de tipo privado** (no disponible en todos los navegadores todavía), simplemente prefijando almohadilla (`#`) al nombre de la propiedad. Sin embargo, hay que tener en cuenta que si queremos dar valor a una propiedad privada desde un método (o constructor) debe estar declarada (o inicializada) en la clase (en cualquier punto de esta):

```js
class Coche {
    #marca;  // declaración del atributo privado
    constructor(marca, modelo, color) {
        this.#marca = marca;  // #marca es realmente privado ahora
        this._modelo = modelo;
        this._color = color;
    }
    get marcaCoche() { return this.#marca; }
    set marcaCoche(marca) { this.#marca = marca; }
    set modeloCoche(modelo) { this._modelo = modelo; }  // write-only property
    get colorCoche() { return this._color; }  // read-only property
}
```
