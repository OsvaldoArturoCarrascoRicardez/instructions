# Replicacion MariadDB Master - Master

### Máquina M1 (master)

- **IP:** 10.1.4.244 - Admcs1234567

### Máquina M2 (slave)

- **IP:** 10.1.4.243 - Admcs1234567

## Contraseña de MariaDB

Al instalar MariaDB en NAS configurar el password

- `Admcs1234567@`

---

## Configuración de Master (nodo 1)

1. Instalar MariaDB y acceder a la consola MySQL:

```bash
mysql -u root -p
```

2. Crear usuario para replicación:

```bash
CREATE USER 'replicate_user'@'%' IDENTIFIED BY 'Admcs12345@';

GRANT REPLICATION SLAVE ON *.* TO 'replicate_user'@'%';

flush privileges;

SHOW MASTER STATUS\G
```

3. Crear la carpeta `logs`:

```bash
cd /volume1/@appdata/MariaDB10

sudo mkdir -m 777 logs
```

4. Crear archivos:

```bash
sudo vim mariadb-bin

sudo vim mariadb-bin.index

sudo vim relay-bin

sudo vim relay-bin.index

```

5. Asignar permisos:

```bash
sudo chmod 777 mariadb-bin

sudo chmod 777 mariadb-bin.index

sudo chmod 777 relay-bin

sudo chmod 777 relay-bin.index
```

6. Crear el archivo de configuración `my.cnf` en `/var/packages/MariaDB10/etc`:

```bash
sudo vim my.cnf
```

Insertar

```ini
[mysqld]
bind-address = 10.1.4.244
server-id=101
report_host=M1
log_bin=/volume1/@appdata/MariaDB10/logs/mariadb-bin
log_bin_index=/volume1/@appdata/MariaDB10/logs/mariadb-bin.index
relay_log=/volume1/@appdata/MariaDB10/logs/relay-bin
relay_log_index=/volume1/@appdata/MariaDB10/logs/relay-bin.index
```

7. Reiniciar el servicio de MariaDB:

```bash
sudo /usr/syno/bin/synopkg restart MariaDB10
```

---

## Configuración de Master (nodo 2)

1. Instalar MariaDB.

2. Crear la carpeta `logs`:

```bash
cd /volume1/@appdata/MariaDB10

sudo mkdir -m 777 logs
```

2. Crear archivos:

```bash
sudo vim mariadb-bin

sudo vim mariadb-bin.index

sudo vim relay-bin

sudo vim relay-bin.index
```

3. Asignar permisos:

```bash
sudo chmod 777 mariadb-bin

sudo chmod 777 mariadb-bin.index

sudo chmod 777 relay-bin

sudo chmod 777 relay-bin.index
```

4. Crear el archivo de configuración `my.cnf` en `/var/packages/MariaDB10/etc`:

```bash
sudo vim my.cnf
```

Insertar

```ini
[mysqld]
bind-address = 10.1.4.243
server-id=102
report_host=M2
log_bin=/volume1/@appdata/MariaDB10/logs/mariadb-bin
log_bin_index=/volume1/@appdata/MariaDB10/logs/mariadb-bin.index
relay_log=/volume1/@appdata/MariaDB10/logs/relay-bin
relay_log_index=/volume1/@appdata/MariaDB10/logs/relay-bin.index
```

7. Reiniciar el servicio de MariaDB:

```bash
sudo /usr/syno/bin/synopkg restart MariaDB10
```

5. Acceder a la consola MySQL:

```bash
mysql -u root -p
```

2. Crear usuario para replicación:

```bash
CREATE USER 'replicate_user'@'%' IDENTIFIED BY 'Admcs12345@';

GRANT REPLICATION SLAVE ON *.* TO 'replicate_user'@'%';

flush privileges;

SHOW MASTER STATUS\G
```

6. Configurar el master (nodo 2):

```bash
stop slave;

CHANGE MASTER TO
MASTER_HOST='10.1.4.244',
MASTER_USER='replicate_user',
MASTER_PASSWORD='Admcs12345@',
MASTER_LOG_FILE='mariadb-bin.000001',
MASTER_LOG_POS=805;

start slave;
```

7. Verificar el estado del esclavo:

```bash
show slave status\G
```

6. Configurar el master (nodo 1):

7. Acceder a la consola MySQL:

```bash
mysql -u root -p
```

6. Insertar:

```bash
stop slave;

CHANGE MASTER TO
MASTER_HOST='10.1.4.243',
MASTER_USER='replicate_user',
MASTER_PASSWORD='Admcs12345@',
MASTER_LOG_FILE='mariadb-bin.000001',
MASTER_LOG_POS=805;

start slave;
```

7. Verificar el estado del esclavo:

```bash
show slave status\G
```

8. Realizar pruebas de replicación escribiendo en el maestro y verificando en el esclavo.
