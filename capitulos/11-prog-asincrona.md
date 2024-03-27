# Programación asíncrona

## Promesas y then()

Las promesas (*promises*) se usan básicamente para código asíncrono. Una promesa es un objeto que lleva asociada una función que realiza una tarea (normalmente asíncrona), y que cuando ha terminado puede hacer dos cosas: resolver (*resolve*) o rechazar (*reject*) la tarea.

Para construir una promesa (objeto de tipo ***Promise***) hay que pasarle una función, la cual recibe dos parámetros: la función de resolución y la de rechazo (que no hay que definir, ya están definidas por la misma promesa):

```js
const myPromise = new Promise((resolve, reject) => {
    // Código de la función
    // Si todo bien:
    resolve(resultado);
    // Si no ha ido bien:
    reject(error);
});
```

La función de la promesa debe terminar llamando o bien a la *callback* ***resolve*** o a la ***reject***, según sea el resultado de la acción. El resultado de la promesa es el valor que se pasa a la función ***resolve*** (a la función ***reject*** se le pasará el valor del error).

Para ejecutar la promesa, se debe llamar al método `then()` de la misma. Esto dejará ejecutando la función, y cuando el resultado esté listo, este resultado se pasará a la *callback* indicada en `then()` como parámetro, la cual recibirá un argumento: el resultado de la promesa.

```js
myPromise.then(resultado => {
    /* acciones a realizar cuando hemos recibido el resultado */
});
```

El método `then()` retorna a su vez una promesa, con lo que pueden encadenarse varios métodos `then()`, uno tras otro. Cada uno de ellos define una función *callback* que recibe como parámetro el valor retornado por la *callback* del `then()` anterior.

De esta forma nos aseguramos que el código dentro de un `then()` no empieza a ejecutarse hasta que el del `then()` anterior no ha retornado un resultado. Pero mientras tanto, el código fuera de la promesa (y la cadena de ***thens***) se puede seguir ejecutando.

La semántica de ***then*** es: se ejecuta primero la *callback* de la promesa; ***then*** (luego) la *callback* del primer `then()`; ***then*** la del siguiente; etc.

El objeto ***Promise*** en sí puede tener tres estados: *pending* (todavía ejecutando), *fulfilled* (resuelto) y *rejected* (rechazado).

Al final de la cadena de ***thens*** se puede encadenar un método `catch()` para el caso que se haya producido un error o rechazo. Allí donde se produzca el error, la ejecución se saltará el resto de la cadena hasta el `catch()`. Este método recibe como parámetro una función *callback* que a su vez recibe como parámetro el error producido.

## async y await

Una función retornará una promesa simplemente añadiéndole la palabra `async`.

```js
async function foo1() { /*...*/ }    // función normal
const foo2 = async () => { /*...*/ }    // arrow function
```

Por otro lado, la palabra clave `await`, **que solo puede usarse dentro de una función** `async`, se usa para indicar que debe pausarse el código (de la función `async`) hasta que la respuesta esté disponible. Es decir, el código posterior a la sentencia `await` no se ejecutará hasta que se haya resuelto esta.

La expresión asociada a una sentencia `await` es, o bien una *promise*, o una función `async`. Sin embargo, `await` no retorna una promesa o una función asíncrona, sino los datos resueltos por dicha promesa o función asíncrona, o un error. En este último caso, para tratar un posible error durante `await`, se debería colocar dicha sentencia dentro de una cláusula `try`.

```js
async function asyncHelloWorld() {
    return "Hello world!";  // podría ser un valor obtenido en una web
}

async function asyncCaller() {
  const result = await asyncHelloWorld();
  console.log(result);   // Hello world!
}

asyncCaller();
```

Como vemos en este ejemplo, la función ***asyncCaller()*** espera (*awaits*) al resultado de la función ***asyncHelloWorld()***, la cual retorna una promesa que resuelve al valor de retorno ***Hello world!***. Es decir, el `return` de una función asíncrona no es el valor que retorna dicha función, sino el valor al que resuelve la promesa que de hecho retorna la función.

## La API fetch

La función `fetch()` retorna directamente una promesa. Se le debe pasar como argumento la *URL* del servidor. Si resuelve bien, resolverá a la respuesta *HTTP* del servidor. Para extraer el contenido *JSON* de la *response*, se debe usar el método `json()` de la misma:

```js
fetch("http://servidor.com")
    .then(respuesta => {
        return respuesta.json();
    })
    .then(resultado => {
        console.log(resultado);
    });
```

En este caso se lanza la petición al servidor. En cuanto se reciben las cabeceras, tenemos listo el valor de ***respuesta*** para ejecutar el primer `then()`. Lo que no está disponible todavía es el cuerpo del *JSON*, con lo que el método `json()` lo que retorna es una promesa (la cual está en estado *pending*). Cuando esta promesa está *fulfilled* porque se ha terminado de leer ya la respuesta completa, se ejecuta el segundo `then()`, cuya *callback* recibe la resolución de la promesa.

Cuando la función *callback* de un `then()` retorna a su vez una promesa, esta es devuelta por el método `then()`. En ese caso, el valor que recibe la *callback* del siguiente `then()` es el resultado de resolución de dicha promesa. Por otro lado, cuando la *callback* de un `then()` retorna un objeto cualquiera, el siguiente `then()` lo interpreta como un valor válido de resolución, y su función *callback* lo recibe como tal.

La función `fetch()` puede recibir un segundo argumento con informaciones específicas para la petición. Por ejemplo:

```js
promesa = fetch("http://servidor.com", {
    method: "GET",
    headers: {
        Accept: "application/json"
    }
});
```

Utilizando `async` y `await`:

```js
async function getData() {
    let respuesta = await fetch("http://servidor.com");
    let datosJson = await respuesta.json();
    return datosJson;
}
```

Ejemplo de envío de datos *JSON* a un servidor, y recepción de una respuesta también *JSON*:

```js
async function postData(unObjeto) {
    const respuesta = await fetch("http://servidor.com", {
        method: "POST",
        headers: {
            "Content-Type": "application/json"
        }
        body: JSON.stringify(unObjeto)
    });
    const jsonResp = await resupesta.json();
}
```

## El objeto respuesta

El objeto respuesta retornado por la función `fetch()` tiene una serie de propiedades que nos pueden ser muy útiles:

- ***body***: el contenido de la respuesta.
- ***headers***: las cabeceras de la respuesta.
- ***ok***: *booleano* indicando si la respuesta es correcta o no.
- ***status***: código de estado de la respuesta.
- ***statusText***: texto correspondiente al código de estado de la respuesta.
- ***json()***: retorna una promesa que se resuelve con un formato *JSON*.
- ***text()***: retorna una promesa que se resuelve con un formato de texto.

## Formularios

Es posible enviar un formulario de forma asíncrona. Para ello definiremos nuestro formulario en *HTML*, y crearemos el objeto formulario mediante la clase ***FormData***. Lo único que habrá que pasarle como argumento al constructor de esta clase será el objeto formulario que deseamos enviar:

```javascript
const datos = new FormData(document.getElementById('mi-formulario'));
```

Una vez tengamos este objeto, lo pasaremos como información adicional a `fetch()`:

```javascript
const respuesta = await fetch("http://servidor.com", {
    method: "POST",
    body: datos
});
```

> Si el método es ***GET***, no se puede hacer así, ya que este método carece de cuerpo (***body***) en la solicitud.

En este caso, todos los campos del formulario serán enviados, mediante el método indicado en ***method***. Observando este mecanismo, podemos inferir que los atributos ***action*** y ***method*** del formulario *HTML* son innecesarios (aunque se pueden escribir, lo indicado en la función `fetch()` tendrá prioridad).

Por otro lado, antes de enviar la solicitud es posible añadir nuevos campos al formulario programáticamente mediante el método `append()`:

```javascript
datos.append('email', 'pepe@servidor.com');
```

Así, no es necesario escribir el formulario en *HTML*. Podríamos construir todo el formulario a partir de un objeto ***FormData*** vacío:

```javascript
let formulario = new FormData();
formulario.append('campo1', 'valor1');
formulario.append('campo2', 'valor2');
// etc.
```
