



# Introducción

## Escenarios
- Un sitio web tradicional:
    - html
    - css
    - javascript
- Una aplicación web (SPA):
    - jade u otra template en vez de html
    - sass o less en vez de css
    - CoffeScript, TypeScript, ES6-7 en vez de js (ES5)


## Task runners
- Necesitamos un *compilador* que nos genere el código que necesita el navegador: html, css y js (ES5)
- Una SPA depende de bastantes librerías, principalmente de js.
    - Necesitamos concatenarlas (evitar múltiples http requests)
- Consumir dependencias y gestionarlas mediante un gestor de paquetes como **npm**
- Optimizar y comprimir imágenes
- Linter de los ficheros para detectar errores
- ...
- Se utilizan Grunt o Gulp y se configuran en base a plugins que se pueden instalar vía npm

## Bundlers
- El problema con Grunt o Gulp viene cuando se utilizan muchos assets:
- Sería útil poder dividirlos en paquetes o bundlers de modo que:
    - Los que no se modifiquen (vendor assets), se separan, de modo que se puedan cachear
    - Determinadas páginas del sitio necesitan assets extra (panel de administración)
- Webpack y Browserify dan esta opción

# Ejemplo de uso

## Aplicación Hola Mundo
- Vamos a crear un Hola Mundo.
- La forma tradicional sería:

```
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Ejercicio</title>
</head>
<body>
 <script>
  var element = document.createElement('h1');
  element.innerHTML = 'Hola Mundo';
  document.body.appendChild(element);
 </script> 
</body>
</html>
```

## Generación Hola Mundo mediante webpack
- Supongamos que el js del ejemplo anterior lo dividimos en dos ficheros:
  - *main.js*:
    - Se encarga de hacer de puente entre los js de mi app (en un futuro componentes) y el html
  - *component.js*:
    - Representa un componente que generará un título para mi página
    - Este componente podría recibir parámetros como el texto del título, el estilo o incluso llamar a otros componentes...
    
## Fichero main
- *main.js*:
```
'use strict';
var component = require('./component.js');
document.body.appendChild(component());
```

## Fichero component
- *component.js*:
```
'use strict';
module.exports = function () {
    var element = document.createElement('h1');
    element.innerHTML = 'Hola Mundo';
    return element;
};
```


## Uso básico de webpack
- Instalamos webpack a nivel global (sudo en linux):
```
sudo npm i webpack -g
```

- Compilamos nuestro main.js:
```
webpack main.js bundle.js
```

- Lo llamamos desde un html:
```
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Ejercicio</title>
</head>
<body>
 <script src='bundle.js'></script> 
</body>
</html>
```

- ¿Y si llamáramos al main.js en vez del bundle.js?

## Opciones de webpack
- ¿Qué hace webpack?
  - Junta todo nuestro js en un solo fichero
  - Divide el js en módulos
  - Carga el módulo inicial

- Para ver toda las posibilidades
```
webpack --help
```

- Por ejemplo:
webpack -w main.js bundle.js // se queda como un servicio
webpack -p main.js bundle.js // minified
webpack -d main.js bundle.js //debug con sourcemap

## Configuración de webpack
- webpack tiene muchas opciones
  - Usaremos un fichero de configuración para personalizarlo
  - Utilizamos node para montar nuestro entorno de desarrollo (plugins para webpack por ejemplo)
  - Instalaremos webpack de forma local a nuestro proyecto

## Generación entorno de desarrollo
- Mediante node
  - Deberá estar instalado node y su gestor de paquetes npm
- Creamos nuestro proyecto
```
mkdir hola-mundo
cd hola-mundo
npm init -y
```
- Se genera el fichero *package.json* con las propiedades del proyecto

# Instalación de Webpack
- Desinstalamos webpack (lo teníamos instalado de forma global):
```
npm remove --save-dev webpack
```
- Instalamos webpack como dependencia de nuestro proyecto:
```
npm install --save-dev webpack
```
- O más corto:
```
npm i -D webpack
```

- Ya tenemos el ejecutable (webpack) para poderlo utilizar. Para ver donde se instala:
```
npm bin
```

## Estructura de nuestro proyecto
- Vamos a crear un Hola Mundo:
- Estructura de nuestro proyecto:
```
/app
  main.js
  component.js
/build
  bundle.js (se creará mediante webpack)
  index.html (a mano o mediante webpack)
package.json
webpack.config.js
```

- *index.html* puede ser:
    - un fichero estático 
    - un fichero [generado dinámicamente](https://www.npmjs.com/package/html-webpack-plugin)

- *main.js* es la entrada a nuestra aplicación
- *component.js* es una dependencia 
- *webpack.config.js* es el fichero de configuración de webpack


## Fichero de configuración de webpack

- *webpack.config.js*
```
var path = require('path');
module.exports = {
    entry: path.resolve(__dirname, 'app/main.js'),
    output: {
        path: path.resolve(__dirname, 'build'),
        filename: 'bundle.js',
    }
};
```

- Generar el fichero indicado en output *build/bundle.js* 
    - a partir del indicado en entry: main.js 
    - Examina sus dependencias y las incluye en el bundle.

## Ejecución
- Ejecutamos webpack:
```
node_modules/.bin/webpack
```
- Creamos el fichero build/index.html que carge el js
- Probamos que muestre *Hola Mundo*
- Este ejemplo y los siguientes los puedes descargar de un [repositorio de GitHub](https://github.com/juanda99/webpack-ejemplos-tutorial)

## Generación index.html de forma dinámica
- Utilizaremos el [plugin HtmlWebpackPlugin](https://www.npmjs.com/package/html-webpack-plugin) para webpack:

```
var path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
module.exports = {
    entry: path.resolve(__dirname, 'app/main.js'),
    output: {
        path: path.resolve(__dirname, 'build'),
        filename: 'bundle.js',
    },
    plugins: [
      new HtmlWebpackPlugin({
        title: 'Hola Mundo'
      })
    ]
};
```

## Añadimos nuestro proceso de compilación al proyecto
- En los scripts del fichero package.json añadimos la compilación:
```
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "webpack"
  },
```

- Ahora podremos ejecutarlo mediante **npm run build**
- No hace falta poner la ruta, ya que npm añade de forma temporal el directorio *node_modules/.bin* al PATH. 

## Añadir CSS
- Añadimos nuestro fichero component.css:
```
h1 {
  color: red;
}
```
- Lo llamamos desde nuestro fichero component.js:
```
'use strict';
require("./component.css");
module.exports = function () {
    var element = document.createElement('h1');
    element.innerHTML = 'Hola Mundo';
    return element;
};
```

## Procesar CSS
- Webpack debe saber importar ficheros css, para ello necesita 2 loaders:
  - **CSS loader**: que importa el fichero CSS y procesa los import y url() que tenga.
  - **Style loader**: que procesa el CSS generado por CSS Loader y lo inserta en nuestra página html.
- Instalamos los módulos:
```
npm i -D css-loader style-loader
```
- Configuramos el módulo en webpack:
```
    module: {
        loaders: [
            { test: /\.css$/, loader: "style-loader!css-loader" }
        ]
    }
```

## Añadir SASS
- Cambiamos nuestro fichero component.css por component.scss y con código propio de Sass:
```
$primary-color: red;
h1 {
  color: $primary-color;
}
```
- Cambiamos el require de component.js para que llame al fichero anterior.

## Procesar SASS
- Instalamos los paquetes necesarios:
```
npm i -D sass-loader node-sass
```
- Añadimos el procesado de ficheros scss desde webpack:
```    
    module: {
        loaders: [
            { test: /\.css$/, loader: "style-loader!css-loader" },
            { test: /\.scss$/, loader: "style-loader!css-loader!sass-loader" }
        ]
    }
```

## Referencias
- http://survivejs.com/webpack/introduction-to-webpack/