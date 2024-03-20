# Instalar MariaDB Galera Cluster en Ubuntu 22.04.2

**Actualizar servidores**

```bash
sudo apt update && sudo apt -y upgrade
sudo vim /etc/hosts
```

Añade lo siguiente de acuerdo a tus ip y nombre de server:

```text
10.1.4.151 mariadb01
10.1.4.152 mariadb02
```

**Instalar MariaDB en todos los nodos**

```bash
sudo apt update

sudo apt -y install mariadb-server mariadb-client

sudo mysql_secure_installation

mysql -u root -p
```

**Habilitar MariaDB en todos los nodos**

```bash
sudo systemctl enable mariadb

sudo systemctl start mariadb

sudo systemctl status mariadb
```

**Abre los puertos necesarios Para Galera, los puertos 3306 (MySQL) y 4567-4568 (Galera Cluster) suelen ser necesarios.**

```sh
sudo ufw allow 3306/tcp
sudo ufw allow 4567:4568/tcp
```

Asegúrate de reiniciar el servicio ufw después de hacer cambios en la configuración:

```sh
sudo systemctl restart ufw
```

**Configurar Galera Nodo (1) - Crear archivo de configuración**

```bash
sudo vim /etc/mysql/conf.d/galera.cnf
```

Añade la siguiente configuración:

```ini
[mysqld]
binlog_format=ROW
default-storage-engine=innodb
innodb_autoinc_lock_mode=2
bind-address=0.0.0.0

# Galera Provider Configuration
wsrep_on=ON
wsrep_provider=/usr/lib/galera/libgalera_smm.so

# Galera Cluster Configuration
wsrep_cluster_name="csdocs_cluster_db"
wsrep_cluster_address="gcomm://mariadb01,mariadb02"

# Galera Synchronization Configuration
wsrep_sst_method=rsync

# Galera Node Configuration
wsrep_node_address="10.1.4.151"
wsrep_node_name="mariadb01_node"
```

**Configurar Galera Nodo 2 (superman)**

```bash
sudo vim /etc/mysql/conf.d/galera.cnf
```

Añade la siguiente configuración:

```ini
[mysqld]
binlog_format=ROW
default-storage-engine=innodb
innodb_autoinc_lock_mode=2
bind-address=0.0.0.0

# Galera Provider Configuration
wsrep_on=ON
wsrep_provider=/usr/lib/galera/libgalera_smm.so

# Galera Cluster Configuration
wsrep_cluster_name="csdocs_cluster_db"
wsrep_cluster_address="gcomm://mariadb01,mariadb02"

# Galera Synchronization Configuration
wsrep_sst_method=rsync

# Galera Node Configuration
wsrep_node_address="10.1.4.152"
wsrep_node_name="mariadb02_node"
```

**Detener el servicio de mariadb en ambos nodos**

```bash
sudo systemctl stop mariadb
```

**Ejecuta en el nodo 1 (batman)**

```bash
sudo galera_new_cluster
```

**Verifica el estatus del nodo (1)**

```bash
mysql -u root -p -e "SHOW STATUS LIKE 'wsrep_cluster_size'"
```

**Activar el segundo nodo**

```bash
sudo systemctl start mariadb
```

**Verifica el estatus del nodo (2)**

```bash
mysql -u root -p -e "SHOW STATUS LIKE 'wsrep_cluster_size'"
```

### **TEST**

**Escriba en el nodo (1)**

```bash
mysql -u root -p -e 'CREATE DATABASE playground;
CREATE TABLE playground.equipment ( id INT NOT NULL AUTO_INCREMENT, type VARCHAR(50), quant INT, color VARCHAR(25), PRIMARY KEY(id));
INSERT INTO playground.equipment (type, quant, color) VALUES ("slide", 2, "blue");'
```

**Lectura y Escritura en el nodo (2)**

```bash
mysql -u root -p -e 'SELECT * FROM playground.equipment;'
mysql -u root -p -e 'INSERT INTO playground.equipment (type, quant, color) VALUES ("swing", 10, "yellow");'
```

**Vuelva a leer en el nodo (1)**

```bash
mysql -u root -p -e 'SELECT * FROM playground.equipment;'
```

**Note:** Esta es una configuración básica para un cluster MariaDB Galera.
