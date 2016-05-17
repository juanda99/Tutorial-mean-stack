# React Native
## Instalación
- Requisitos previos: node y npm
- [Instalar Android Studio](http://developer.android.com/sdk/installing/index.html)
- 
- Instalación:
```
npm install -g react-native-cli
```

## Android Studio
- Lo podemos [instalar de forma manual](http://developer.android.com/sdk/installing/index.html):
    - Descargar software
    - Instalar dependencias como Java y librerías varias
- O mediante Ubuntu make (antes Ubuntu developer tool center)
    - cli para descargar e instalar la última versión de las herramientas más populares de desarrollo
    - Gestiona las dependencias

## Instalación mediante umake
- Instalación de umake:
```
sudo add-apt-repository ppa:ubuntu-desktop/ubuntu-make
sudo apt-get update
sudo apt-get install ubuntu-make
```
- Uso de umake (ver lista de software)
```
umake -h
# por ejemplo en IDES
umake ide -h
```
- Instalación de Android Studio:
```
umake android # o umake android android-studio
```

## Configuración Android Studio
- Según la web de [React-native](https://facebook.github.io/react-native/docs/getting-started.html#content)

- Instalo watchman de lo recomendado
    - Tenemos que tener previamente los siguientes paquetes:
```
    sudo apt-get install automake
    sudo apt-get install python-dev 
```

## Actualización variables de entorno
- Fichero ($HOME/.config/fish/fish.config para fish shell):
```
juanda@cifejd ~/AwesomeProject> cat ~/.config/fish/config.fish 
set -x PATH $PATH  ~/Android/Sdk/tools ~/Android/Sdk/platform-tools
set -x ANDROID_HOME /home/juanda/Android/Sdk/
juanda@cifejd ~/AwesomeProject> 
```

## Test de funcionamiento
- Antes de hacerlo, hay que levantar un device: *android avd*

