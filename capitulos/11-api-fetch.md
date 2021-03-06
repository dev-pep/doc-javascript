# La API Fetch

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

La semántica de ***then*** es: se ejecuta primero la *callback* de la promesa; ***then*** (luego) la *callback* del primer `then()`; ***then*** la del siguiente; etc.

El objeto ***Promise*** en sí puede tener tres estados: *pending* (todavía ejecutando), *fulfilled* (resuelto) y *rejected* (rechazado).

Al final de la cadena de ***thens*** se puede encadenar un método `catch()` para el caso que se haya producido un error o rechazo. Allí donde se produzca el error, la ejecución se saltará el resto de la cadena hasta el `catch()`. Este método recibe como parámetro una función *callback* que a su vez recibe como parámetro el error producido.

## Ejemplo: fetch()

La función `fetch()` retorna directamente una promesa. Se le debe pasar como argumento la *URL* del servidor. Si resuelve bien, retornará una respuesta *HTTP* del servidor. Para extraer el contenido *JSON* de la *response*, se debe usar el método `json()` de la misma:

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

## async y await

En ocasiones, la cadena de métodos `then()` puede complicar el código, y hacerlo menos legible. Existe una nueva forma de trabajar con promesas que lo hace mucho más claro.

La palabra clave `await` se usa para indicar que debe esperarse (asíncronamete) al resultado de la expresión asíncrona correspondiente. Es importante indicar que solo puede utilizarse esta palabra clave dentro de funciones que estén definidas como asíncronas, y ello se hace mediante otra palabra clave: `async`:

```js
async function foo1() { /*...*/ }    // función normal
const foo2 = async () => { /*...*/ }    // arrow function
```

Así, el ejemplo anterior podría reescribirse como:

```js
async function getData() {
    let respuesta = await fetch("http://servidor.com");
    let datosJson = await respuesta.json();
    return datosJson;
}
```

La expresión asíncrona asociada a una sentencia `await` puede ser, o bien una *promise*, o una función `async`.

Ejemplo de envío de datos a un servidor:

```js
async function postData(unObjeto) {
    const response = await fetch("http://servidor.com", {
        method: "POST",
        headers: {
            "Content-Type": "application/json"
        }
        body: JSON.stringify(unObjeto)
    });
    const jsonResponse = await response.json();
}
```
