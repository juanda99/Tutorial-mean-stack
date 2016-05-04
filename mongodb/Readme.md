## ¿Qué es MongoDB?
- Base de datos noSQL
    - No es relacional
    - No tiene un lenguaje SQL
- Open Source
- Orientada a documentos
    - Tablas -> Colecciones
    - Filas -> Documentos
- Muy buena para datos sin estructura fija


## Relacional vs orientada a documentos

facturas_cab (id_factura, id_cliente, fecha, num_factura, importe_total)
facturas_det (id_factura_det, concepto, importe)

vs

factura(id_factura, cliente_nombre, cliente_dni, num_factura, importe_total, array (concepto, importe))

## Versiones de MongoDB
- Community Edition 
- Enterprise


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

# Operaciones CRUD en MongoDB

## Colecciones
- Cada colección la forman un conjunto de documentos
- Cada documento tiene entidad por sí mismo
- Tienen un **esquema dinámico**
    - Cada documento puede tener campos distintos

## Mongo Shell
- Mediante el comando **mongo** podemos entrar al intérprete de MongoDB
- Nos comunicaremos con MongoDB a través del shell **usando JavaScript**
- Los documentos se almacenan en BSON (similar a JSON)
```
var alumno1 = {
    "nombre: "Pepe",
    "apellido": "García",
    "edad": 20
}
```

## Primeros comandos
- Ver bases de datos y colecciones:
```
show databases / show dbs
show collections
```
- Ver base de datos actual
```
db
```
- Usar la base de datos colegio (si no existe, la crea)
```
use colegio
```
- Ayuda
```
help
db.help()
```
## Inserción de datos
- La colección alumnos, si no existe, se crea
- alumno es el objeto json definido anteriormente
```
db.alumnos.insert(alumno1)
```
## Buscar datos
- Utilizamos el método find() de las colecciones
- Se diferencia mayúsculas y minúsculas
```
db.alumnos.find()
db.alumnos.find({"nombre": "Pepe"})
```
- Observa que a los elementos creados, MongoDB les asocia un **atributo _id**
    - Utiliza el tipo de datos ObjectId
    - Lo habitual es dejar que los gestione MongoDB

## Documentos complejos

```
var alumno2 = {
 "nombre": "Juan",
 "apellido": "Marcos",
 "edad": 20,
 "fechaAlta":  new Date (2015, 8, 13),
 "modulos": ["Diseño web", "Desarrollo web cliente", "Entornos de desarrollo"],
 "calificaciones": {"Bases de datos": 5, "Lenguaje de marcas": 8, "Programación": 7}
}
db.alumnos.insert(alumno2)
```
- Calificaciones es un **documento embebido**
- La fecha es el 13 de Septiembre, ¡los meses en js empiezan en 0!

## Más búsquedas
- Alumnos que cursan Diseño Web:
```
db.alumnos.find({"modulos": "Diseño web"})
```
- Alumnos que tienen un 10 en Programación:
```
db.alumnos.find({"calificaciones.Programacion": 10})
```

## Búsquedas dinámicas:
- Queremos buscar en base a un campo que recibimos mediante variable:
```
var modulo = "Diseño web"
var campo = "modulos"
var query={}
query[campo]=modulo
db.alumnos.find(query)
```


## Validación de datos
- Los datos no se validad
    - Se puede insertar cualquier cosa en cualquier campo
    - Si el campo no existe se crea
- Las únicas validaciones son:
    - Que no exista otro documento con el mismo id
    - Que no haya errores de sintaxis
    - Que el documento no pese más de 16Mb

## Borrar documentos
- El método **remove()** borra todos los documentos de la colección que cumplen la query:
- no se usa *delete* porque es una palabra reservada de js
```
db.alumnos.remove({"nombre": "Pepe"})
```

## Actualizar documentos
- El **método update** tiene dos parámetros:
    - La query
    - El parámetro de actualización (van con $)
```
db.alumnos.update(
    {"nombre": "Juan", "apellido": "Marcos"}, 
    {"$set": {"calificaciones.Programación": 9}}
)
```
- En el siguiente caso, cambiaríamos un documento por otro (puede ser útil si importamos datos):
```
db.alumnos.update(
    {"nombre": "Juan", "apellido": "Marcos"}, 
    {"calificaciones.Programación": 9}
)
```
- Los update actualizan el primer documento que cumple la query.
- Para hacer varias actualizaciones a la vez, usamos un tercer parámetro:
```
db.alumnos.update(
    {"fechaAlta":  new Date (2015, 8, 13)}, 
    {"$set": {"fechaAlta": new Date (2016, 8, 13)}},
    {"multi": true}
)
```

## Actualiación con $inc
- Creamos una colección que cuente los accesos a las fichas de los alumnos
- Actualizamos los accesos incrementando una unidad:
```
db.logs.update(
	{"nombre": "Juan", "apellido": "Marcos"},
	{"$count": {"accesos": 1}}
) 
```

## upsert
- La primera vez, no habría actualización, porque la query inicial no selecciona ningún documento
- Habría que hacer un **upsert**
- Si el documento no existe se crea con los valores de la query y el parámetro de update
```
db.logs.update(
	{"nombre": "Juan", "apellido": "Marcos"},
	{"$count": {"accesos": 1}},
	{"upsert": true}
) 
```
- Si ejecutamos de nuevo la query, hará una modificación en vez de un upsert.

## $unset
- Sirve para eliminar campos en documentos
- Eliminemos el campo fechaAlta en todos los documentos:
```
db.alumnos.update(
	{},
	{"$unset": {"fechaAlta":""}},
 	{"multi": true}
)
```
- La query vacía {} es para seleccionar todos los documentos de la colección
- El valor de fechaAlta solo sirve para que el json tenga una sintaxis correcta
- El parámetro multi es necesario para que actualice más de un registro

## $rename
- Para cambiar el nombre de un campo
- Funcionamiento similar a $unset

## Actualización de un array
- Vamos a actualizar *Diseño web* a *Diseño de interfaces web*:
```
{
 "nombre": "Juan",
 "apellido": "Marcos",
 "edad": 20,
 "fechaAlta":  new Date (2015, 8, 13),
 "modulos": ["Diseño web", "Desarrollo web cliente", "Entornos de desarrollo"],
 "calificaciones": {"Bases de datos": 5, "Lenguaje de marcas": 8, "Programación": 7
}
```
- Problemática (eliminaría el resto de módulos):
```
db.alumnos.update(
	{"modulos": "Diseño web"},
	{"$set": {"modulos": "Diseño de interfaces web}}
```

- Solución (utilizamos el índice del array):

```
db.alumnos.update(
	{"modulos": "Diseño web"},
	{"$set": {"modulos.0": "Diseño de interfaces web}}
)
```

## Operador posicional
- Si actualizamos varios documentos:
    - Hara falta el parámetro **multi**
    - El módulo que queremos actualizar no estará siempre en la primera posición
- La query debe tener el valor que queremos cambiar
```
db.alumnos.update(
	{"modulos": "Diseño web"},
	{"$set": {"modulos.$": "Diseño de interfaces web}},
 	{"multi": true}
)
```



## Otros operadores de actualización
- **$max**: Actualiza si el nuevo valor es mayor que el actual (o si está vacío)
- **$min**: Actualiza si el nuevo valor es menor que el actual (o si está vacío)
- **$mul**: Múltiplica el campo por el valor especificado (o 0 si vacío)
- **$pop**: para quitar valores de un arrray (por delante o detrás)
- **$push**: para añadir valores a un array (por el final)
- **$addToSet**: añade el valor al final del array si no existe previamente
- **$pull**: para quitar determinados valores de un array 


## Operadores de comparación
- $lt, $gt, $lte, $gte, $ne

```
db.alumnos.find(
	{"edad": {"$gt": 20}}
)
db.alumnos.find(
	{"edad": {"$gt": 20, "$lt": 40	}}
)
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


