
## Instalación de node

```
curl -sL https://deb.nodesource.com/setup_5.x | sudo -E bash -
sudo apt-get install -y nodejs
sudo apt-get install build-essential
```

Comprobamos que esté instalado:

```
npm -v
node -v
```


## Instalación y configuración de git

```
sudo apt-get update
sudo apt-get install git
```

- Mi configuración, necesaria para cada commit que haga:

git config --global user.name "Your Name"
git config --global user.email "youremail@domain.com"

- Opcionalmente el editor (si no me gusta el que hay por defecto):

git config --global core.editor vi

## Niveles de configuración de git

- Git tiene 3 niveles de configuración, cada nivel sobreescribe el anterior:
    - Para todos los usuarios: */etc/gitconfig*
    - Para un usuario: *~/.gitconfig*  (opción --global)
    - Para un repositorio: *.git/config* 

- Para ver los parámetros configurados:

```
git config --list
```

## Configuración de GitHub
- Nos registramos en [Github](https://github.com/) 
- Accedemos a nuestra cuenta
- Vamos a los settings y asociamos una ssh-key
    - Evitaremos introducir usuario/contraseña en cada *git push*
- Como creamos nuestra ssh-key:

```
ssh-keygen
```
- Copiaremos el contenido de ~/.ssh/id_rsa.pub a una nueva clave ssh en GitHub

## Instalación de fish

- Algunos prefieren zsh
- Otros son fieles a bash
- ¿Por qué fish?
    - Gran experiencia de usuario "out of the box":
        - Sugiere comandos
        - Autocompleta
        - [Personalizaciones](https://github.com/justinmayer/tacklebox)

```
sudo apt-add-repository ppa:fish-shell/release-2
sudo apt-get update
sudo apt-get install fish
chsh -s `which fish`
```

## Instalación de Sublime Text 3

- [Instalación y configuración de Sublime Text 3 en Ubuntu 14.04 para desarrollo Web](http://www.formandome.es/linux/instalacion-y-configuracion-de-sublime-text-3-en-ubuntu-14-04-para-desarrollo-web/)

```
sudo add-apt-repository ppa:webupd8team/sublime-text-3 
sudo apt-get update
sudo apt-get install sublime-text-installer
```

## Instalación de Google Chrome
- [Desde la web](https://www.google.com/chrome/browser/desktop/index.html)

## Instalación de Adobe Brackets
- [Web de referencia](http://brackets.io/)
- Es un editor exclusivamente para web (js, html, css)
- [Más información](http://www.formandome.es/linux/instalacion-y-configuracion-de-brackets-en-ubuntu-14-04/)
- 
## Instalación de Gnome Shell
- [Como instalarlo y configurarlo](http://www.formandome.es/linux/configuracion-inicial-de-ubuntu-14-04/)
- Instalaremos también un terminal: Guake

## Instalación de MongoDB
- [Instalaremos primero mongodb](https://docs.mongodb.com/master/tutorial/install-mongodb-on-ubuntu/):
```
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv EA312927
echo "deb http://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.2 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.2.list
sudo apt-get update
sudo apt-get install -y mongodb-org
```

- El servicio se levanta como otros servicios de Linux: 
```
sudo service mongod start
```

- Y para entrar a su consola, mediante **mongo**, o mediante algún gui como por ejemplo [Robomongo](https://robomongo.org/)

