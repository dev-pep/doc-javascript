# Programación asíncrona

## Promesas y then()

Las promesas (*promises*) se usan básicamente para código asíncrono. Una promesa es un objeto que lleva asociada una función que realiza una tarea (normalmente asíncrona), y que cuando ha terminado puede hacer dos cosas: resolver (*resolve*) o rechazar (*reject*) la tarea.

Para construir una promesa (objeto de tipo ***Promise***) hay que pasarle como parámetro una función, la cual recibe dos parámetros: las funciones *callback* de resolución y de rechazo (que no hay que definir, ya están definidas por la misma promesa):

```js
const myPromise = new Promise((resolve, reject) => {  // función arrow en este caso
    // Código de la función
    // Si todo bien:
    resolve(resultado);
    // Si no ha ido bien:
    reject(error);
});
```

La función de la promesa debe terminar llamando o bien a la *callback* ***resolve*** o a la ***reject***, según sea el resultado de la acción. El resultado de la promesa es el valor que se pasa como argumento a la función ***resolve*** (a la función ***reject*** se le pasará como argumento el valor del error).

Para ejecutar la promesa, se debe llamar al método `then()` de la misma. Este método recibe un argumento solo: una función *callback* asociada, que recibirá y tratará el resultado resuelto por la promesa.

Así, la llamada a `then()` dejará ejecutando la función propia de la promesa, y cuando el resultado de esta función esté resuelto, dicho resultado se pasará a la función *callback* asociada al método `then()`, la cual recibirá un argumento: el resultado resuelto por la promesa.

```js
myPromise.then(resultado => {  // función arrow con un solo parámetro: resultado
    /* acciones a realizar con ese resultado */
});
```

Es decir, `then()` no hace más que esperar y recoger el resultado de la promesa, para tratarlo mediante su propia función asociada.

En caso de que deseemos encadenar varios métodos `then()`, uno tras otro (por tener todos menos el último un comportamiento asíncrono), dicho método retorna a su vez una promesa. Cada uno de ellos define su propia función *callback* para tratar el resultado resuelto por la función del `then()` anterior.

El último ***then*** de la cadena no debe retornar ningún valor. Aunque técnicamente puede hacerlo, nadie recogería ese resultado, con lo que sería inútil retornarlo.

Para resolver el resultado, la función asociada a un método `then()` solo tiene que retornar un valor mediante `return`. Si el valor que retorna es un objeto promesa, es decir, si "resuelve a una promesa", en realidad resolverá a lo que dicha promesa resuelva.

Si por ejemplo definimos una promesa con su cadena de `then()`, y a continuación otra promesa con su cadena, independiente de la primera, al no ser bloqueante el código de la promesa, ambas cadenas se ejecutarían de forma concurrente. Igual si hay más de dos. Si embargo si después de una cadena hay código síncrono, y después otra cadena, el código síncrono se ejecutaría tras la creación de la primera promesa (antes de ejecutar el primer `then()` de esta), pero la segunda cadena no se ejecutaría (ni ningún `then()` de la primera cadena) hasta que el código síncrono hubiese terminado. Cuando este código termina, se crea la segunda promesa, y las dos cadenas prosiguen concurrentemente.

> Para que un ***then*** realice un rechazo, puede levantar una excepción, ya sea porque el código la levanta por sí mismo, o mejor mediante `throw`.

Por lo explicado hasta ahora, vemos que el código de la función asociada a un ***then*** no empieza a ejecutarse hasta que el ***then*** anterior no ha resuelto. Pero mientras tanto, el código fuera de la promesa (y fuera de la cadena de ***thens***) sigue ejecutándose.

Al final de la cadena de ***thens*** se puede encadenar un método `catch()` para el caso que se haya producido alguna excepción en algún `then()` o en la promesa inicial (esto incluye una llamada a la función de rechazo por parte de dicha promesa: esa llamada es una excepción en toda regla que se debe tratar obligatoriamente). Allí donde se produzca el error, la ejecución se saltará el resto de la cadena hasta el `catch()` final. Este método recibe como parámetro una función *callback* que a su vez recibe como parámetro el error producido.

Una promesa tiene tres posibles estados: pendiente, resuelta o rechazada.

## async y await

Si a una función se le añade la palabra `async`, automáticamente retornará una promesa. El valor que retorne dicha función (mediante `return`) será simplemente el valor de resolución de la promesa.

Del mismo modo que sucedía con `then()`, una función asíncrona que resuelva (con `return`) a un objeto promesa, en realidad resolverá a lo que dicha promesa resuelva.

```js
async function foo1() { /*...*/ }    // función normal
const foo2 = async () => { /*...*/ }    // función arrow
```

Por otro lado, la palabra clave `await`, **que solo puede usarse dentro de una función** ***async***, se usa para indicar que la función debe suspenderse hasta que el objeto que estamos esperando (un objeto esperable) se resuelva o rechace, en cuyo momento la ejecución estará lista para proseguir. Es decir, el código posterior a la sentencia `await` (dentro de la misma función) no se ejecutará hasta que se haya resuelto esta.

La expresión asociada a una sentencia `await` es, o bien una *promise*, o una llamada a una función `async` (que es, funcionalmente, igual que una promise). Sin embargo, `await` no retorna una promesa o una función asíncrona, sino que recoge y retorna el valor de retorno de dicha promesa o función asíncrona, es decir, la resolución de esta.

Es decir, al encontrarse con una sentencia `await`, la función actual (`async function`) se suspenderá y quedará en una cola, donde **seguirá ejecutándose en segundo plano** hasta llegar a su resolución o rechazo (excepción). La función se ejecuta estando en la cola, incluyendo posibles sentencias `await`. En todo caso, hasta que no se resuelva (`return`) o rechace (excepción), seguirá en cola. Cuando se resuelva o rechace, ya puede ser retomada la función que esperaba esta tarea, para seguir con su ejecución en primer plano.

Dicha cola va recibiendo tareas suspendidas y estas van saliendo a medida que terminan su ejecución (cuando les toca el turno).

En caso de error durante la ejecución de la promesa (en segundo plano), se debe tratar mediante cláusulas `try` / `catch`, con lo que se debería colocar la sentencia `await` dentro de una cláusula `try`.

```js
async function asincrona1() {
    return "Buenos días";  // podría ser un valor obtenido en una web
}

async function asincrona2() {  // llamará a la asincrona1()
  const resultado = await asincrona1();
  console.log(resultado);   // Buenos dias
}
```

La función ***asincrona2*** debe ser declarada como asíncrona, puesto que tiene en su código una cláusula `await`. En cuanto a la función ***asincrona1*** debe declararse a su vez asíncrona para que se pueda incluir en una sentencia `await`.

## La API fetch

La función `fetch()` está especializada en peticiones (*requests*) a un servidor.

La función retorna directamente una promesa. Se le debe pasar como argumento la *URL* del servidor. Si resuelve bien, resolverá a la respuesta *HTTP* del servidor.

> La resolución de `fetch()` es un objeto del ***tipo respuesta*** (*response*).

Si queremos extraer el contenido *JSON* del objeto respuesta, se debe usar el método `json()` de dicho objeto. Este método retorna una promesa que resuelve al contenido *JSON*:

```js
fetch("http://servidor.com")
    .then(respuesta => {
        return respuesta.json();
    })
    .then(resultado => {
        console.log(resultado);
    });
```

En este caso se lanza la petición al servidor. En cuanto se reciben las cabeceras, tenemos listo el valor de ***respuesta*** para ejecutar el primer `then()`. Lo que no está disponible todavía es el cuerpo del *JSON*, con lo que el método `json()` lo que retorna es una promesa. El segundo método `then()` recibe el resultado resuelto de la promesa retornada por `json()`.

Existen otros métodos similares del objeto respuesta, como `text()` o `blob()` (*binary large object*). Ambos retornan una promesa.

> Lo que obtendremos finalmente mediante el método `json()` no es un string en formato JSON, sino un objeto construído a partir de la respuesta JSON. Si lo que queremos es el string JSON tal cual, debemos llamar al método `text()`.

La función `fetch()` puede recibir un segundo argumento con informaciones específicas para la solicitud (objeto *request*). Por ejemplo:

```js
promesa = fetch("http://servidor.com", {
    method: "GET",
    headers: {
        Accept: "application/json"
    }
});
```

### Ejemplos utilizando async y await

```js
async function getData() {
    let respuesta = await fetch("http://servidor.com");
    let datosJson = await respuesta.json();
    return datosJson;
}
```

Envío de datos *JSON* a un servidor, y recepción de una respuesta también *JSON*:

```js
async function postData(unObjeto) {
    const respuesta = await fetch("http://servidor.com", {
        method: "POST",
        headers: {
            "Content-Type": "application/json"
        }
        body: JSON.stringify(unObjeto)
    });
    const jsonResp = await respuesta.json();
}
```

### El objeto respuesta

El objeto respuesta retornado por la función `fetch()` tiene una serie de propiedades que nos pueden ser muy útiles:

- ***body***: el cuerpo de la respuesta.
- ***headers***: las cabeceras de la respuesta.
- ***ok***: booleano que indica si la respuesta es correcta o no.
- ***status***: código de estado de la respuesta.
- ***statusText***: texto correspondiente al código de estado de la respuesta.

### Formularios

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
