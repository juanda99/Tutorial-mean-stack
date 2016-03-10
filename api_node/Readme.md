# API en Node.js
## Primeros pasos
- npm configurado y git configurado
- Crear repositorio en GitHub
- Clonar repositorio
- Ejecutar npm init
- Crear la estructura de la aplicación, por el momento una carpeta app donde iremos guardando el código propio de la aplicación
- Instalar express mediante uno de los comandos siguientes:

```
npm install --save express
npm i -S express
```

- Creamos el fichero app/server.js donde pondremos el código necesario para testear una API muy básica para probar Express:

```
var express = require('express'); //llamamos a Express
var app = express();                

var port = process.env.PORT || 8080;  // establecemos nuestro puerto

// para establecer las distintas rutas, necesitamos instanciar el express router
var router = express.Router();              

//establecemos nuestra primera ruta, mediante get.
router.get('/', function(req, res) {
    res.json({ mensaje: '¡Hola Mundo!' });   
});

// nuestra ruta irá en http://localhost:8080/api
// es bueno que haya un prefijo, sobre todo por el tema de versiones de la API
app.use('/api', router);

// iniciamos nuestro servidor
app.listen(port);
console.log('API escuchando en el puerto ' + port);
```

- Iniciamos nuestro API Server mediante el comando 
```
node app/server.js
```

Probamos que la API funcione mediante http://localhost:8080. 

- Para homogeneizar, vamos a crear un script en nuestro fichero package.json, de modo que podamos arrancar nuestra API mediante ```npm start```
```
"start": "node app/server.js"
```

## Recibir parámetros
- Cuando el router recibe una petición, podemos observar que ejecuta una función de callback *function (req, res)*:
    -  El parámetro **req** representa la petición (request) 
    -  El parámetro **res** representa la respuesta (response), que en el caso anterior hemos codificado en json.
- A menudo la petición se hará enviando algún parámetro adicional. Hay varias posibilidades:
    - Mediante la url: se recogerán mediante req.param.nombreVariable
    - Mediante post en http hay dos posiblidades:
        - application/x-www-form-urlencoded
        - multipart/form-data
- En peticiones post, escogeremos x-www-form-urlencoded si no se envía una gran cantidad de datos (normalmente ficheros).  Más info: http://stackoverflow.com/questions/4007969/application-x-www-form-urlencoded-or-multipart-form-data

## Parámetros por url
- Vamos a mandar un parámetro *nombre* a nuestra api, de modo que nos de un saludo personalizado.

router.get('/:nombre', function(req, res) {
    res.json({ mensaje: '¡Hola' + req.params.nombre });   
});

## nodemon
- Es un wrapper de node, para reiniciar el servidor de apis cada vez que detecte modificaciones.

```
npm i -D nodemon
```

- Cada vez que ejecutemos **npm start** ejecutaremos nodemon en vez de node. Habrá que cambiar el script en el fichero *package.json*:

```
"start": "nodemon app/server.js"
```

## eslint
- Usaremos eslint para formatear nuestro código.

```
npm i -D eslint
```

- Como lo instalo en local, para ejecutarlo, necesito darle el path:
```
./node_modules/.bin/eslint --init
? How would you like to configure ESLint? Answer questions about your style
? Are you using ECMAScript 6 features? No
? Where will your code run? Node
? Do you use JSX? No
? What style of indentation do you use? Spaces
? What quotes do you use for strings? Single
? What line endings do you use? Unix
? Do you require semicolons? No
? What format do you want your config file to be in? JSON
Successfully created .eslintrc.json file in /home/juanda/api_node_express/ejercicio3-nodemon-eslint
```

