



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

## Generación entorno mediante webpack
- Utilizamos node para montar nuestro entorno de desarrollo
```
mkdir hola-mundo
cd hola-mundo
npm init -y
```
- Se genera el fichero *package.json* con las propiedades del proyecto
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
  bundle.js (automatically created)
  index.html
package.json
webpack.config.js
```

- *index.html* puede ser:
    - un fichero estático 
    - un fichero [generado dinámicamente](https://www.npmjs.com/package/html-webpack-plugin)

- *main.js* es la entrada a nuestra aplicación
- *component.js* es una dependencia 
- *webpack.config.js* es el fichero de configuración de webpack

## Fichero main
- *app/main.js*:
```
'use strict';
var component = require('./component.js');
document.body.appendChild(component());
```

## Fichero component
- *app/component.js*:
```
'use strict';
module.exports = function () {
    var element = document.createElement('h1');
    element.innerHTML = 'Hola Mundo';
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
    }
};
```

- Generar el fichero indicado en output *build/bundle.js* 
    - a partir del indicado en entry: main.js 
    - Examina sus dependencias y las incluye en el bundle.


## Referencias
- http://survivejs.com/webpack/introduction-to-webpack/