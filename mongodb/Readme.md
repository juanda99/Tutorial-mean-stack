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



