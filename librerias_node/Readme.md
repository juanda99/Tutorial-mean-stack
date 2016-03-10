# Microlibrería
- Ventajas
    - Poco código, se entiende y modifica con facilidad
    - Reusable
    - Fácil hacer tests
- Desventajas
    - Tienes que gestionar muchas dependencias
    - Control de versiones de todas ellas

# Mi librería

- Obtiene una marca de cerveza y sus características
- Utilizaremos el fichero cervezas.json
- [Instalaremos el paquete pretty.json](https://packagecontrol.io/packages/Pretty%20JSON) en Sublime Text	
    - Usando la paleta de comandos (pretty...) podremos formatear, validar o hacer un minify del json
    
# Control de versiones
- Utilizaremos git como control de versiones
- Utilizaremos github como servidor git en la nube para almacenar nuestro repositorio:
    - Haz login con tu usuario (o crea un usuario nuevo)
    - Crea un nuevo repositorio en GitHub
    - Sigue las indicaciones de GitHub para crear el repositorio en local y asociarlo al repositorio remoto (GitHub)

# Instalación de node
- Lo más sencillo es [instalar mediante el gestor de paquetes](https://nodejs.org/en/download/package-manager/)

```
curl -sL https://deb.nodesource.com/setup_5.x | sudo -E bash -
sudo apt-get install -y nodejs
```

- Comprobamos que esté correctamente instalado

```
node -v
npm -v
```
# npm
- Es el gestor de paquetes de node
- Podemos buscar paquetes que nos interese instalar
- Podemos publicar nuestra librería :-)
    - Debemos crear un usuario en https://www.npmjs.com/


# Configuración de npm
- Cuando creemos un nuevo proyecto nos interesa que genere automaticamente datos como nuestro nombre o email
- Ver [documentación para su configuación](https://docs.npmjs.com/) o mediante consola (```npm --help```)	:
- Mediante ```npm config --help``` vemos los comandos de configuración
- Mediante ```npm config ls -l``` vemos los parámetros de configuración

```bash
npm set init-author-name pepe
npm set init-author-email pepe@pepe.com
npm set init-author-url http://pepe.com
npm set init-license MIT
npm adduser
```

- Los cambios se guardan en el fichero $HOME/.npmrc
- ```npm adduser``` genera un authtoken = login automático al publicar en el registro de npm

## Versiones en node
- Se utiliza [Semantic Versioning](http://semver.org/)

```
npm set save-exact true
```

- Las versiones tienen el formato MAJOR.MINOR.PATCH
- Cambios de versión:
    - MAJOR: Cambios en compatibilidad de API,
    - MINOR: Se añade funcionalidad. Se mantiene la compatibilidad.
    - PATCH: Se solucionan bug. Se mantiene compatibilidad.

- ¡Puede obligarnos a cambiar el MAJOR muy a menudo! 






	


