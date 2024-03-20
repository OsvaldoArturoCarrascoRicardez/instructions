# Replicacion MariadDB Master - Slave

### Máquina M1 (master)

- **IP:** 10.1.4.245 -  Admcs1234567

### Máquina M2 (slave)

- **IP:** 10.1.4.243 - Admcs1234567

## Contraseña de MariaDB

Al instalar MariaDB en NAS configurar el password

- `Admcs1234567@`

---

## Configuración de Master

1. Instalar MariaDB y acceder a la consola MySQL:

```bash
mysql -u root -p
```

2. Crear usuario para replicación:

```bash
create user replicate_user@10.1.4.243 identified by 'Admcs12345@';
grant replication slave on *.* to replicate_user@10.1.4.243;
flush privileges;
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
bind-address = 10.1.4.245
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

## Configuración de Slave

1. Instalar MariaDB.

2. Crear archivos:

```bash
sudo vim mariadb-bin

sudo vim mariadb-bin.index
```

3. Asignar permisos:

```bash
sudo chmod 777 mariadb-bin

sudo chmod 777 mariadb-bin.index
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
expire-logs-days=10
read_only=1
```

7. Reiniciar el servicio de MariaDB:

```bash
sudo /usr/syno/bin/synopkg restart MariaDB10
```

5. Acceder a la consola MySQL en el esclavo:

```bash
mysql -u root -p
```

6. Configurar el esclavo:

```bash
stop slave;

change master to
   master_host='10.1.4.245',
   master_user='replicate_user',
   master_password='Admcs12345@',
   master_log_file='mariadb-bin.000001',
   master_log_pos=330;

start slave;
```

7. Verificar el estado del esclavo:

```bash
show slave status\G
```

8. Realizar pruebas de replicación escribiendo en el maestro y verificando en el esclavo.
