



# Introducción a webpack

## ¿Para qué?
- Un sitio web tradicional:
    - html
    - css
    - javascript
- Un sitio web actual:
    - jade en vez de html
    - sass o less en vez de css
    - CoffeScript, TypeScript, ES6-7 en vez de js
- Necesitamos un *compilador* que nos genere el código que necesita el navegador


## Elegir un compilador
- Hay muchos:
    - Grunt
    - Gulp
    - Browserify
    - Webpack
- Webpack es el último en llegar y el más potente



## Un ejemplo sencillo:
- Estructura de nuestro proyecto:
```
/app
  main.js
  component.js
/build
  bundle.js (automatically created)
  index.html
package.json
webpack.config.js
```

- *index.html* puede ser:
    - un fichero estático 
    - un fichero [generado dinámicamente](https://www.npmjs.com/package/html-webpack-plugin)

- *app/main.js*:
```
'use strict';
var component = require('./component.js');
document.body.appendChild(component());
```

- *app/componen.js*:
```
'use strict';
module.exports = function () {
    var element = document.createElement('h1');
    element.innerHTML = 'Hello world';
    return element;
};
```

## Fichero de configuración de webpack

- *webpack.config.js*
```
var path = require('path');
module.exports = {
    entry: path.resolve(__dirname, 'app/main.js'),
    output: {
        path: path.resolve(__dirname, 'build'),
        filename: 'bundle.js',
    },
};
```
- Generar el fichero indicado en output *build/bundle.js* 
    - a partir del indicado en entry: main.js 
    - Examina sus dependencias y las incluye en el bundle.


## Referencias
- http://survivejs.com/webpack/introduction-to-webpack/