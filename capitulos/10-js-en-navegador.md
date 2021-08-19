# JavaScript en el navegador

## Programación web

Para incluir código fuente en un documento *HTML*, se utiliza la etiqueta `<script>` (y `</script>`). Se puede incluir directamente el código entre estas etiquetas, aunque es frecuente utilizar el atributo `src` de las mismas para definir un archivo externo (mediante ruta relativa al archivo *HTML*, o absoluta). En todo caso es obligatorio incluir la etiqueta de cierre.

Para cargar un módulo, se puede hacer con `<script>` con el atributo `type="module"`. Si ese módulo a su vez importa otros módulos, también serán cargados.

Antes se utilizaba también el atributo `language="javascript"`, pero actualmente ya no es necesario (no hay otro). Tampoco es necesario especificar el atributo `type` (a no ser que queramos cargar un módulo).

*JavaScript* puede inyectar *HTML* mediante `document.write()`, aunque se considera mal estilo. Pero como es posible que exista esa sentencia, el navegador debe ejecutar todo el *script* antes de seguir renderizando la página, lo cual implica descargarlo y ejecutarlo, lo cual puede ralentizar la presentación. Para evitarlo se puede dar el atributo `async` o `defer`, en cuyo caso, el navegador sigue renderizando la página aunque no haya ejecutado el *script*. Estos atributos informan al navegador de la inexistencia de `document.write()`; el primero de ellos lo ejecuta en cuanto está disponible, mientras que el segundo lo ejecuta cuando se ha terminado de parsear el *HTML* entero. Si se indican los dos, `async` es preferente.

```html
<script defer src="archivo.js"></script>
```

Por defecto, los **módulos** se cargan con el modo `defer`, aunque se puede indicar `async`.

En el caso de `defer`, es útil si accedemos al *DOM* y no queremos colocar el *script* al final del ***body***, ya que si lo colocamos al principio y no especificamos `defer`, los accesos al *DOM* fallarán, ya que no estarán presentes en el documento todavía cuando intentemos el aceso.

### Document Object Model (DOM) y el objeto global

El *DOM* es una representación jerárquica del documento *HTML* completo. Cada etiqueta se representa mediante una instancia de su tipo carácteristico, y los atributos de la etiqueta se corresponden con las propiedades de dicha instancia.

Todos los archivos *JavaScript* (*scripts* y módulos) corriendo en el mismo documento tienen acceso al **objeto global**. Si un módulo o *script* define un cambio en ese objto, será visible a todos los demás. En este objeto están también la biblioteca estándar (como `parseInt()`, el objeto `Math`, etc.). Las propiedades de este objeto se acceden directamente. Una de las propiedades del mismo es `window`, que es una referencia al objeto global en sí (de tipo `Window`). Otra es `document`, que representa el documento *HTML* actual (de tipo `Document`).

Los módulos definen elementos en su propio *scope*, pero los *scripts* no módulos, que definen elementos en el *top level* lo definen en el ámbito global, con lo que es accesible desde otros *scripts* sin necesidad de exportación. En este caso, la definición de una variable mediante `var`, o la definición de una función, se añade al objeto global, con lo que, a parte de ser accesible directamente, es accesible a través de `window`. Las declaraciones con `const`, `let` y `class` no se añaden al objeto global.

Si una página tiene un frame insertado (`<iframe>`), los *scripts* que contiene tienen acceso a un *DOM* distinto.

## Eventos

El método para registrar un manejador de eventos es con el método `addEventListener()`. Se puede también definir en *HTML* (o añadirlo con *Javascript* como propiedad en el elemento adecuado):

```html
<button onclick="console.log('Thank you');">Please Click</button>
```

El método `addEventListener()`, definido para todos los elementos del documento, toma tres argumentos. El primero es el nombre del tipo de evento (p.e. ***click***, ***mouseover***, ***mouseout***, ***keydown***, etc.). El segundo es la función manejadora. En este ejemplo se utilizan las dos formas para registrar un manejador de eventos:

```html
<button id="mybutton">Click me</button>
<script>
    let b = document.querySelector("#mybutton");
    b.onclick = function() { console.log("Thanks for clicking!"); };
    b.addEventListener("click", () => { console.log("Thanks again!"); });
</script>
```

En este caso, cuando se hace clic sobre el botón se ejecutará primero la que se ha registrado primero, y luego la función *arrow* (siempre en orden de registro), ya que no se sobrescriben, sino que se van añadiendo, a no ser que se intente registrar una función ya registrada para ese objeto y evento (no tendrá efecto). En este sentido, pasar varias veces una función *inline* o *arrow* sí tendrá efecto, ya que se trata de distintas referencias a funciones (aunque sean iguales).

Para eliminar uno de los registros se usa el método `removeEventListener()`, al que se le pasan estos dos parámetros. Por lo tanto, si hemos usado una función anónima (sin nombre), no podremos eliminarla.

Cuando se invoca un *event handler*, se le pasa un argumento, que es un objeto `Event`. Propiedades del mismo:

- ***type***: tipo de evento.
- ***target***: objeto sobre el que ocurre el evento (objeto donde se originó el mismo).
- ***currentTarget***: en eventos que se propagan, objeto en el que se registró el *handler* actual.
- ***timeStamp***: *timestamp* del evento.
- ***isTrusted***: verdadero si el evento lo generó el navegador, y falso si se generó por código *JavaScript*.
Eventos específicos tendrán propiedades adicionales con información.

Dentro del manejador, `this` es el objeto al que pertenece tal manejador. Pero si es una *arrow function*, `this` tiene el mismo significado que en el *scope* donde se define dicha *arrow function*.

Un evento se puede **propagar**. Cuando se produce un evento sobre un objeto del documento actual, y tras la invocación de los *handlers* del objeto (si los hay para ese evento), el evento sigue para arriba en la jerarquía (*"bubble" up*), invocándose a continuación los *handlers* del padre (si los tiene para ese evento), y siguiendo hacia arriba. La mayoría de eventos *bubble up*. El evento ***load*** también lo hace, pero se detiene tras `Document` (no se propaga a `Window`).

Con anterioridad a la invocación de todos estos métodos manejadores, de abajo a arriba, existe una fase anterior, llamada fase de **captura**. En esta fase se ejecutan los llamados *capturing handlers* de los objetos, en orden descendente, desde el objeto `Window` hacia abajo (*window*, *document*, *body*,...). En el elemento *target* no se ejecutan los *capturing handlers*.

El manejador no debería retornar ningún valor.

Si queremos parar el comportamiento por defecto de un formulario, lo aconsejable es llamar al método `preventDefault()` del objeto `Event` (escuchando el evento ***submit***).

Si deseamos detener la propagación del evento, se llamará al método `stopPropagation()` del objeto evento. Esto hará que se invoquen el resto de manejadores del objeto actual (no en el caso de `stopImmediatePropagation()`), pero tras esto, no se propagará el evento hacia arriba (o hacia abajo en caso de *capturing handlers*).

Por otro lado, a la hora de registrar el manejador, existe un tercer argumento, opcional, que podemos pasar a `addEventListener()`. Se trata de un objeto que puede tener hasta tres propiedades: ***capture*** (para marcar el manejador como *capturing handler*), ***once*** (para eliminar el manejador tras su primera ejecución) y/o ***passive*** (indica que el manejador no invoca a `preventDefault()`, y si lo hace queda sin efecto). Si se omite este argumento, su valor por defecto es ***false***, equivalente a especificar sus propiedades como falsas. Si se indican algunas de estas propiedades, las omitidas se consideran ***false*** por defecto. Pasar ***true*** directamente equivale a pasar `{capture: true}`.

En cuanto a `removeEventListener()`, también se puede pasar este tercer argumento, aunque solo es relevante la propiedad ***capture***: si se ha registrados como *capturing handler*, para eliminar el manejador habrá que pasarle también esa propiedad como ***true***.

## Acceso al DOM

El contenido del documento (*DOM*) es accesible a través del objeto `document`. Para seleccionar un elemento del documento, se pueden usar los métodos `querySelector()` y `querySelectorAll()`, que seleccionan a partir de **selectores *CSS***. La primera de estas funciones retorna el primero de los elementos que coinciden con el selector que se pasa como argumentos (o ***null*** si no lo encuentra). La segunda retorna un objeto (*array-like*) con todas las coincidencias.

Estos métodos se invocan desde un objeto (elemento del documento) y van buscando jerarquía abajo. Al contrario que `closest()`, que busca jerarquía arriba.

Con el método `getElementById()` se selecciona un elemento por ID, y con `getElementsByClassName()` por el nombre de la clase (retorna un *array-like* con los elementos encontrados). También disponemos de métodos como `getElementByName()` o `getElementsByTagName()`.

Un elemento es un objeto de tipo `Element`, que son todos descendientes del objeto `Document`. Para un objeto `Element`, encontramos las propiedades `parentNode` (elemento padre, o documento), `children` (*array-like* con los elementos hijo), `childrenElementCount`, `firstElementChild`, `lastElementChild`, `nextElementSibling`, `previousElementSibling`. Cuando no procede, retornan ***null***.

Estos métodos ignoran los objetos que no son `Element`, como texto y comentarios. Si queremos que se ignore nada, hay que usar las propiedades `childNodes`, `firstChild`, `lastChild`, `nextSibling`, `previousSibling`. Para saber el tipo de un nodo concreto, podemos usar la propiedad `nodeType`: 9 (documento), 1 (elemento), 3 (texto), 8 (comentarios). `nodeValue` retorna el contenido de un nodo texto o comentario. `nodeName` retorna la etiqueta *HTML* en mayúsculas.

Los métodos de tipo `getElementBy*` que retornan más de un elemento, retornan una ***HTMLCollection*** de **elementos**, mientras que `querySelectorAll()` retorna una ***NodeList*** de nodos.

Las funciones de selección se pueden llamar desde el objeto `document`, o desde cualquier otro. Por ejemplo:

```js
let divs = document.querySelectorAll("div");
let div1 = divs.getElementById("div_1");
```

Para acceder a los atributos de un elemento, disponemos de los métodos `getAttribute()`, `setAttribute()`, `hasAttribute()`, `removeAttribute()`. Pero los elementos que representan elementos *HTML* tienen los atributos directamente accesibles mediante propiedades.

El atributo `class` contiene estilos *CSS*. En *JavaScript* podemos acceder directamente al atributo ***class*** del elemento, podemos acceder a su contenido mediante la propiedad ***classList***, con todas las clases a las que pertenece el elemento en un objeto tipo *array*; este objeto dispone de métodos para manipular estas clases:

- `add()` añade la clase que le pasamos.
- `remove()` elimina la clase especificada.
- `contains()` retorna verdadero o falso, dependiendo de si contiene la clase indicada.
- `toggle()` añade la clase si no la tiene, o la elimina en caso contrario.

Se puede leer el contenido de un elemento *HTML* mediante la propiedad `innerHTML` (*read/write*). Mediante `outerHTML`, obtenemos algo parecido, solo que incluyendo las etiquetas de apertura y cierre del elemento. `textContent` es el contenido del elemento como texto plano (sin etiquetas; si se le dan etiquetas *HTML* serán texto visible en la página).

Se pueden crear y eliminar nuevos nodos con el método `createElement()` de la clase `Document`. Para añadir y quitar elementos en cualquier elemento, se usa `append()` y `prepend()` (solo objetos `Element`).

```js
let paragraph = document.createElement("p");  // Crea un elemento <p> vacío
let emphasis = document.createElement("em");  // Crea un elemento <em> vacío
emphasis.append("World");  // Añade texto al <em>
paragraph.append("Hello ", emphasis, "!");  // Añade texto y <em> a <p>
paragraph.prepend("¡");  // Añade más texto al principio de <p>
paragraph.innerHTML;  // evalúa a "¡Hello <em>World</em>!"
document.body.append(paragraph);  // lo hacemos visible añadiendo el nodo al cuerpo del documente
```

Los métodos para añadir elementos aceptan cualquier número de argumentos, que pueden ser nodos, o *strings* (en este caso será tomado como texto plano). Para insertar contenido, pero ni al principio ni al final, se usa `before()` y `after()`, que funcionan sobre cualquier tipo de nodo.

Si queremos duplicar un nodo, se usa el método `cloneNode()`, al que se le pasa ***true*** para que copie todo el contenido.

### Location e History

A través de la propiedad `location` (tanto del documento como de la ventana) se puede acceder a cosas como la *URL*, el protocolo, puerto, *host*, enlace de la página, etc.

El objeto `history` guarda el historial de navegación. Este dispone de los métodos `back()` y `forward()`. Se puede avanzar varios pasos a la vez mediante el método `go()` al que se le pasa un entero con el número de pasos (negativo para pasos hacia atrás).
