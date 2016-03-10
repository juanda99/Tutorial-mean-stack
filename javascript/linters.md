# Analizadores de código (linters)
- Herramientas que realizan la lectura del código fuente
    - Detectan errores/warnings de código
        - Variables sin usar o no definida, llave sin cerrar...
    - Detectan fallos de estilo
        - Comillas dobles en vez de simples, espacios en vez de tabulaciones...

# JSLint
- JSLint es un analizador online de código javaScript creado por Douglas Crockford 
- Los criterios evaluados corresponden a los que marcó su creador 
    - Demasiado estricto
    - No es configurable o extensible

# JSHint
    - Fork de JSLint
    - El objetivo de JSHint es no imponer un convenio particular
    - La gente utiliza diferentes estilos y convenciones.
    - El linter debe adaptarse al desarrollador y no al revés

# JSCS
    - Solo para verificar el estilo del código
    - Tiene muchas reglas. No hace falta defirlas, podemos utilizar un [preset](https://github.com/jscs-dev/node-jscs/tree/master/presets)

# ESLint
    - El último en llegar (2013), recoge lo bueno de los anteriores
    - Podemos [fijar nuestras reglas](http://eslint.org/docs/rules/) e incluso luego cambiar nuestro estilo (--fix)
    - Y además tiene soporte para ES6 y para JSX (que se usa en React)
    - Será el que usemos :-)

# ESLint: Instalación
- Instalar nodejs
- Instalar ESLint mediante **sudo npm install -g eslint**
- Se instala en /usr/local/bin/eslint y se puede utilizar desde la CLI
- Otra opción es instalarlo de forma local para cada proyecto

# ESLint: Configuración
- [Guía de configuración](http://eslint.org/docs/user-guide/configuring) y [reglas]((http://eslint.org/docs/rules/)
- Mediante el fichero JSON *.eslintrc* 
{
  "env": {
    //enable nodejs environment
    "node": 1,
    //enable browser environment
    "browser": 1
  },
  "globals": {
    //place settings for globals here, such as
    "exampleGlobalVariable": true
  },
  "rules": {
    //enable ESLint rules, such as
    "eqeqeq": 1
  },
  "plugins": [
    //you can put plugins here
  ]
}

# Configuración de reglas
- Las reglas pueden tener 3 valores:
    - 0: Desactiva la regla
    - 1: Genera un warning (pero el código no hace un exit)
    - 2: Genera un error y el código hace un exit 1

# Ejemplo de configuración de plugin
- Vamos a poner una [configuración específica para React](https://www.npmjs.com/package/eslint-plugin-react)
- Primero hay que instalar el plugin:
npm install eslint-plugin-react
- Luego se configura .eslintrc:

{
  "plugins": [
    "react"
  ]
}

# Mi configuración es .eslintrc
- extend sirve para compartir configuraciones

{
  "parser"  : "babel-eslint",
  "extends" : [
    "standard",
    "standard-react"
  ],
  "env"     : {
    "browser" : true
  },
  "globals" : {
    "__DEV__"      : false,
    "__PROD__"     : false,
    "__DEBUG__"    : false,
    "__DEBUG_NEW_WINDOW__" : false,
    "__BASENAME__" : false
  },
  "rules": {
    "semi" : [2, "never"],
    "react/jsx-boolean-value": 0,
    "max-len": [2, 130, 2]
  }
}