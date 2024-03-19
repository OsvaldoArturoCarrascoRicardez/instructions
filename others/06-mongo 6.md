# Instalacion y Configuracion MONGO 6

### **`MONGO`**

Aquí tienes una guía paso a paso para la instalación de MongoDB en Ubuntu:

1. **Actualizar e instalar paquetes necesarios**:

```bash
sudo apt update

sudo apt install wget curl gnupg2 software-properties-common apt-transport-https ca-certificates lsb-release autoconf g++ make openssl libssl-dev libcurl4-openssl-dev pkg-config libsasl2-dev php8.1-dev
```

2. **Importar la clave pública de MongoDB**:

```bash
wget -qO - https://www.mongodb.org/static/pgp/server-6.0.asc | sudo tee /etc/apt/trusted.gpg.d/mongodb-org-6.0.asc
```

3. **Crear el archivo de repositorio de MongoDB**:

```bash
sudo vim /etc/apt/sources.list.d/mongodb-org-6.0.list
```

Pegar la siguiente línea dentro del archivo:

```
 deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/6.0 multiverse
```

4. **Actualizar los repositorios**:

```bash
sudo apt-get update
```

5. **Instalar MongoDB**:

```bash
sudo apt-get install -y mongodb-org
```

6. **Crear el directorio para MongoDB**:

```bash
sudo mkdir /data
cd /data
sudo mkdir db
```

7. **Detener cualquier instancia de MongoDB en ejecución**:

```bash
 sudo pkill -f mongod
```

8. **Iniciar MongoDB**:

```bash
sudo systemctl start mongod
```

9. **Verificar el estado de MongoDB**:

```bash
sudo systemctl status mongod
```

10. **Habilitar MongoDB para que se inicie automáticamente al arrancar**:

```bash
sudo systemctl enable mongod
```

11. **Acceder a la consola de MongoDB**:

```bash
mongosh
```

#### Para permitir el acceso remoto a MongoDB, debes realizar los siguientes pasos:

1. **Editar el archivo de configuración de MongoDB**:

   Abre el archivo de configuración de MongoDB en un editor de texto. El archivo suele estar en `/etc/mongod.conf`.

```bash
sudo vim /etc/mongod.conf
```

2. **Configurar la dirección IP y el puerto de escucha**:

   Dentro del archivo de configuración, busca la línea que especifica `bindIp`. Puedes comentar esta línea para permitir que MongoDB escuche en todas las interfaces de red o especificar la dirección IP de tu servidor. También asegúrate de que el puerto `27017` esté habilitado para aceptar conexiones remotas.

   Ejemplo para permitir todas las conexiones:

```ini
net:
   port: 27017
   bindIp: 0.0.0.0
```

3. **Reiniciar el servicio de MongoDB**:

   Después de realizar los cambios en el archivo de configuración, reinicia el servicio de MongoDB para que los cambios surtan efecto.

```bash
sudo systemctl restart mongod
```

4. **Configurar el firewall**:

   Si estás utilizando un firewall, como UFW, asegúrate de permitir el tráfico entrante en el puerto `27017` para que MongoDB pueda recibir conexiones remotas.

```bash
sudo ufw allow 27017/tcp
```

5. **Configurar nuevo usuario**:

Accede a la consola de MongoDB:

```bash
mongosh
```

Dentro de la consola de MongoDB:

```bash
use admin
db.createUser(
  {
    user: "root",
    pwd: "Admcs1234567",
    roles: [ { role: "root", db: "admin" } ]
  }
)
```

```bash
exit
```

Reinicia el servidor de MongoDB:

```bash
sudo systemctl restart mongod
```
