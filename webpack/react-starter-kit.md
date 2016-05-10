# ESLint: Instalación
- Instalar nodejs
- Instalar ESLint mediante **sudo npm install -g eslint**
- Se instala en /usr/local/bin/eslint y se puede utilizar desde la CLI
- Otra opción es instalarlo de forma local para cada proyecto, preferible, nos gusta el sandbox :-)
```
npm i --save-dev eslint
```

# ESLint: Configuración
- [Guía de configuración](http://eslint.org/docs/user-guide/configuring) y [reglas]((http://eslint.org/docs/rules/)
- Mediante el fichero JSON *.eslintrc*, por ejemplo: 
```
{
  "env": {
    "node": true,
    "browser": true
  },
  "globals": {
    //place settings for globals here, such as
    "exampleGlobalVariable": true
  },
  "rules": {
    //enable ESLint rules, such as
    "semi" : [2, "never"]
  },
  "plugins": [
    //you can put plugins here
  ]
}
```

# Configuración de reglas
- Las reglas pueden tener 3 valores:
    - 0: Desactiva la regla
    - 1: Genera un warning (pero el código no hace un exit)
    - 2: Genera un error y el código hace un exit 1

- Por ejemplo en el .eslintrc anterior el editor nos marcará como error los ; a final de linea

# extend
- Lo más cómodo es utilizar eslint configurado ya por alguien, y luego hacer nuestras pequeñas modificaciones mediante rules.
  - Si hacemos un PR a repositorios de Airbnb, Google... deberemos ser fieles a su guía de estilos
  - Utitlizaremos la guía de Airbnb porque está muy documentada:
  https://www.npmjs.com/package/eslint-config-airbnb-base
  - La podemos modificar, por ejemplo quitando los ; al final de las líneas.

# Instalación configuración de Airbnb
- Lo más cómodo con el propio ejecutable de eslint:
```
./node_modules/.bin/eslint --init
```
- Elegimos:
  - Use a popoular style guide
  - AirBnB
  - JSON

- Nos creará el fichero .eslintrc.json y dos entradas de dependencias en el package.json:
```
"eslint-config-airbnb": "^9.0.1",
"eslint-plugin-react": "^5.1.1",
```

# Babel
- Compila código JavaScript ES5
- Necesitamos las siguientes entradas en el package.json:
```
    "babel-core": "^6.0.20",
    "babel-eslint": "^4.1.3",
    "babel-loader": "^6.0.1",
    "babel-preset-es2015": "^6.0.15",
    "babel-preset-react": "^6.0.15",
    "babel-preset-stage-0": "^6.0.15",
```