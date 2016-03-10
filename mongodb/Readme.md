## MongoDB
- Versiones Community Edition o Enterprise


## Ventajas de la versión Enterprise
- MongoDB Management Service (backup y monitorización)
- Monitorización SNMPg
- Kerberos or LDAP como alternativa a uso de contraseña o certificados
- Interfaz gráfica [MongoDB compass](https://www.mongodb.com/products/compass) (hay [alternativas libres](https://docs.mongodb.org/ecosystem/tools/administration-interfaces/#third-party-open-source-tools)  como [Robomongo](https://robomongo.org/)
MongoDB compass - a GUI tool to visualize data structures (but there are free alternatives for that).
- **La versión community no tiene limitaciones de RAM o almacenamiento**

## Instalación MongoDB Community Edition
- Solo 64 bits

```
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv EA312927
echo "deb http://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.2 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.2.list
sudo apt-get update
sudo apt-get install -y mongodb-org
```

- Arranque y parada:

```
sudo service mongod start
sudo service mongod stop
sudo service mongod restart
```

# Almacenamiento en MongoDB

## Storage Engines
- Responsable del almacenamiento de los datos en memoria y en disco
- WiredTiger es el storage engine por defecto en MongoDB 3.2 (disponible desde la versión 3.0)
    - Concurrencia optimística
    - Concurrencia a nivel de documento
    - Checkpoints cada 60s o 2GBytes de data
    - Si queremos recuperar datos intermedios, necesitamos journaling

## Journaling
- Instancias independientes:
    - Evita perdida de datos si se muere el proceso de MongoDB entre checkpoints.
- Replica sets
    - El proceso de replica puede dar garantías en la recuperación de datos, sin uso de journaling

## Uso de memoria
- MongoDB utiliza **WiredTiger cache** y **filesystem cache**
- Uso de WiredTiger cache: 
    - 1GB o 60% de la RAM menos 1GB (lo que sea mayor)
    - Evita dimensionarlo por encima de estos valores
- Filesystem cache: usa toda la que haya disponible


# Seguridad en MongoDB

## Autenticación
- Mediante el método db.auth()
- Varios métodos de autenticación:
    - MONGODB-CR
        - Usuario, contraseña y bbdd
        - Método por defecto hasta la aparición de SCRAM-SHA1
    - SCRAM-SHA-1
        - Por defecto desde MongoDB 3
        - Mejoras respecto de MONGODB-CR como:
            - Encriptación superior (SHA1 vs MD5)
            - Autenticación bidireccional: el servidor es quien dice ser :-
    - Autenticación mediante certificados X.509
        - Requiere una conexión segura TLS/SSL.
        - Uso de certificados en vez de usuario y contraseña
    - Otras opciones: 
        - LDAP
        - Kerberos

## Usuarios
- Los usuarios se añaden mediante ```db.CreateUser()```
- A los usuarios se le asignan roles. 
- Los roles conceden privilegios para 
    - Acceder a recursos (database, collection, cluster..)
    - Ejecutar acciones (find, insert, remove, update...)
- Los roles se pueden revocar



## Habilitar Client Access Control
- [Standalone environment](https://docs.mongodb.org/manual/tutorial/enable-authentication/)
    - Habrá que crear un usuario con el **role userAdmin** o **userAdminAnyDatabase** en la base de datos **admin**
- [Replica Sets o shared clusters](https://docs.mongodb.org/manual/tutorial/enable-internal-authentication/)
    - Para permitir autenticación interna

## Configura usuarios y roles
- Necesitamos el usuario, la contraseña (dependiendo del tipo de autenticación) y los roles.
- Ejemplo:
```
use reporting
db.createUser(
    {
      user: "reportsUser",
      pwd: "12345678",
      roles: [
         { role: "read", db: "reporting" },
         { role: "read", db: "products" },
         { role: "read", db: "sales" },
         { role: "readWrite", db: "accounts" }
      ]
    }
)

```

# Uso de replicas

## ¿Qué son las réplicas?
- Grupo de instancias mongod que mantienen el mismo conjunto de datos
- Ventajas de múltiples copias de los datos en distintos servidores de bbdd:
    - Redundancia
    - Aumenta la disponibilidad de los datos
    - Puede aumentar la velocidad de lectura o pueden funcionar simplemente como backup

## Arquitectura de las replicas
- Cada instancia de mongod es un nodo
- Habrá un único nodo primario, uno o varios secundarios y opcionalmente un nodo árbitro
- Las escrituras se hacen exclusivamente en el nodo primario, y se replican a los nodos secundarios
- Si el nodo primario pierde la comunicación durante más de 10 segundos con el resto:
    - Un nodo secundario pasa a ser primario
    - Se utiliza un protocolo de replicación para su elección


# Sharding

## ¿Qué es el sharding?
- Método para almacenar datos en varias máquinas
- ¿Cuándo se utiliza?
    - Entornos con un conjunto de datos muy grandes
        - Superior a la capacidad de almacenamiento de un solo equpo
    - Entornos con un throughput elevado
        - Penaliza a la CPU
        - Además si el conjunto de datos es mayor que la RAM del sistema provoca estrés en el I/O de disco
 
## ¿Cómo escalar un sistema?
- Nos quedamos sin CPU, disco o RAM. ¿Qué podemos hacer?
- Escalado vertical
    - Añadimos CPU, RAM o Disco Duro
        - Ojo, puede ser muy caro
        - Los proveedores en la nube pueden ofrecer solo instancias "pequeñas"
        - ¡Estamos limitados!
- Escalado horizontal (sharding)
    - Se dividen los datos en varios servidores o **shards**
    - Cada shard es una base de datos física independiente.
    - El conjunto de todos los shards hacen la base de datos lógica

## Arquitecura para sharding en MongoDB
- MongoDB soporta sharding configurando un **sharded cluster**
    - **Shards** para almacenar los datos (en producción un shard será un replica set)
    - **Query routers**: instancias mongo para redirigen al shard que corresponda
    - **Config servers**: 
        - Almacenan los metadatos del cluster.  
        - El query router utiliza sus metadatos para obtener el shard al que redirigir cada petición

## Particionamiento de datos
- Se realiza a nivel de colección (shard key)


