# Características de JavaScript

# JavaScript es Single Thread
- Nuestro código se ejecuta en un solo hilo
- Evitamos [típicos errores de la programación multihilo](http://stackoverflow.com/questions/499634/how-to-detect-and-debug-multi-threading-problems)
    - Dificiles de detectar
    - Dificiles de replicar



# LLamadas síncronas o bloqueantes
- Hasta que no acaba una instrucción, no empieza la siguiente
    - [Ver ejemplo](https://jsbin.com/fuzofi/edit?html,console,output)
    - No se ejecuta más JavaScript, en según que casos puede parecer que esté "colgado"
- Normalmente no es problema:
    - Las instrucciones se ejecutan con rapidez (equipos rápidos)
- Pero a veces sí:
    - Llamadas a API
    - Lectura de disco
    - ....

```
<!DOCTYPE html>
<html>
<head>
  <script src="https://code.jquery.com/jquery-1.11.3.js"></script>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width">
  <title>JavaScript es single thread</title>
</head>
<body>
  <button>Pulsa aquí </button>
  <script>
    $('button').click(function () {
      var answer = prompt("¿Cómo te llamas?");
      console.log(answer);  
    });
    $('button').click(function () {
      console.log("¡Bienvenido");  
    });
  </script>
</body>
</html>
```

## JavaScript es asíncrono
- Se pude ejecutar una instrucción antes de que acabe la anterior, [Ver ejemplo](https://jsbin.com/hukenok/edit?html,console,output)
- Análisis del ejeemplo:
    - En el event handler hay dos funciones:
        - La primera es asíncrona y no evita que otro código se ejecute en el navegador:
            - ```console.log ("Petición realizada")
            - O la propia renderización en el navegador (css o html)
        - La segunda es una función de callback
            - No se ejecuta hasta que acaba la función asíncrona
- ¡JavaSript no es single thread!
    - Hay un hilo para la ejecución de nuestro software
    - Otro hilo para la gestión interna (avisa de que la llamada asíncrona ha terminado)


``` 
<!DOCTYPE html>
<html>
<head>
  <script src="https://code.jquery.com/jquery-1.11.3.js"></script>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width">
  <title>JavaScript: llamadas asíncronas</title>
</head>
<body>
  <button>Pulsa aquí </button>
  <script>
    $('button').click(function() {
      $.get('http://ask.com'), function(data) {
        console.log("Petición contestada", data);
      });
      console.log ("Petición realizada")
    })
  </script>
</body>
</html>
```

## JavaScript en el servidor: Node.js
- En el servidor Node.js también es single-threaded
- Al contrario que en el navegador, encontramos muchas llamadas asíncronas: 
    - Llamadas a APIs
    - Lectura y escritura de ficheros
    - Ejecución de cálculos en el servidor
    - ....
- Llamadas síncronas en servidor serían fatales:
    - ¡Bloqueariamos las conexiones al servidor hasta que acabase la instrucción bloqueante!

## CallbackHell
- Imagina que guardamos un registro de los accesos de los usuarios a nuestra app:

```
trackUser =  function(userId) {
  users.findOne({userId: userId}, function(err, user) {
    var logIn = {userName: user.name, when: new Date};
    logIns.insert(logIn, function(err, done) {
      console.log('wrote log-in!');
      });
  });
```

- Tenemos 3 funciones anidadas en una simple operación.
- Esto es lo que se conoce como [callback hell](https://strongloop.com/strongblog/node-js-callback-hell-promises-generators/)

## Evitar el callback en el navegador
- Mediante el [uso de promesas](https://www.promisejs.org/)
- Se trata de escribir código asíncrono con un estilo síncrono.
- Opciones más actuales:
    - Generators / Yields (ES6)
    - Async / Await (ES7)
    - El soporte de ES6 en node es limitado (--harmony) y también en el navegador => TRANSPILERS (babel)
- Ver [comparativa de métodos asíncronos](https://thomashunter.name/blog/the-long-road-to-asyncawait-in-javascript/)

# Debug en node:

# babel-node-debug index.js
- Nos muestra el código en ES6
- node-inspector's node-debug command with babel-node!
# Debug en node.js

# 1ª Opción: Built in debugger

- Se ejecuta mendiante:

```node debug app.js```

- El debugger hará un breackpoint en la primera linea de debug que encuentre (console.log por ej)
- Comandos de debug:
    - continue: cont, c
    - step: next, n
    - step in: step, s
    - step out: out, o
- En cualquier momento se puede abrir el REPL y asignar o inspeccionar variables

# 2ª Opción: Mediante [Node Inspector](https://github.com/node-inspector/node-inspector)
- Tenemos una GUI similar a la de las Chrome Developer Tools
- Se instala mediante ```npm install node-inspector``` 
- Habrá que ejecutar dos comandos:
    - *node-debug*: lanzará el debugger (o *node-inspector*)
    - *node --debug app.js*: ejecutaremos nuestra aplicación y la capturará el debugger
- Para que se capture nuestra aplicación desde la primera línea:
    - *node --debug-brk app.js*
- 




# 3ª Opción: Mediante [Iron Node](https://github.com/s-a/iron-node)

