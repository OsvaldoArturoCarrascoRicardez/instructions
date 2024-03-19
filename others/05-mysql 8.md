# Instalacion y Configuracion MySQL 8

### **`MySQL 8`**

#### Paso 1: Actualiza el índice local de paquetes

```bash
sudo apt update
```

#### Paso 2: Instala MySQL

```bash
sudo apt install mysql-server
```

Una vez completada la instalación, el servicio de MySQL se iniciará automáticamente. Para verificar que el servidor MySQL esté en funcionamiento, escribe:

```bash
sudo systemctl status mysql
```

La salida debería mostrar que el servicio está habilitado y en ejecución.

#### Paso 3: Asegurar MySQL

```bash
sudo mysql_secure_installation
```

Se le pedirá que configure el PLUGIN VALIDATE PASSWORD , que se utiliza para comprobar la fortaleza de las contraseñas de los usuarios de MySQL y mejorar la seguridad:

```bash
Securing the MySQL server deployment.

Connecting to MySQL using a blank password.

VALIDATE PASSWORD COMPONENT can be used to test passwords
and improve security. It checks the strength of password
and allows the users to set only those passwords which are
secure enough. Would you like to setup VALIDATE PASSWORD component?

Press y|Y for Yes, any other key for No: y
```

Hay tres niveles de política de validación de contraseñas, bajo, medio y fuerte. Pulsa `y` si quieres configurar el plugin de validación de contraseñas o cualquier otra tecla para pasar al siguiente paso.

Si ha decidido utilizar el componente, el script le pedirá que seleccione el nivel de complejidad y fortaleza de la contraseña. Generalmente es bueno elegir un nivel medio o fuerte: `1`

```bash
There are three levels of password validation policy:

LOW    Length >= 8
MEDIUM Length >= 8, numeric, mixed case, and special characters
STRONG Length >= 8, numeric, mixed case, special characters and dictionary                  file

Please enter 0 = LOW, 1 = MEDIUM and 2 = STRONG: 1
```

#### Iniciar sesión como root

A continuación, se le pedirá que elimine el usuario anónimo, restrinja el acceso del usuario root a la máquina local, elimine la base de datos de prueba y recargue las tablas de privilegios. Deberías responder `y` a todas las preguntas.

```bash
Remove anonymous users? (Press y|Y for Yes, any other key for No) : y
Success.

Normally, root should only be allowed to connect from
'localhost'. This ensures that someone cannot guess at
the root password from the network.

Disallow root login remotely? (Press y|Y for Yes, any other key for No) : y
Success.

By default, MySQL comes with a database named 'test' that
anyone can access. This is also intended only for testing,
and should be removed before moving into a production
environment.

Remove test database and access to it? (Press y|Y for Yes, any other key for No) : y
 - Dropping test database...
Success.

 - Removing privileges on test database...
Success.

Reloading the privilege tables will ensure that all changes
made so far will take effect immediately.

Reload privilege tables now? (Press y|Y for Yes, any other key for No) : y
Success.

All done!
```

Iniciar sesión como root
Puede interactuar con el servidor MySQL desde la línea de comandos utilizando la utilidad cliente MySQL, que se instala como una dependencia del paquete del servidor MySQL.

En MySQL 8.0, el usuario root es autenticado por el plugin auth_socket por defecto.

El plugin auth_socket autentica a los usuarios que se conectan desde el localhost a través del archivo socket Unix. Esto significa que no puede autenticarse como root proporcionando una contraseña.

Para iniciar sesión en el servidor MySQL como usuario root, escriba:

```bash
sudo mysql
```

Aparecerá el intérprete de comandos de MySQL, como se muestra a continuación:

```bash
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 10
Server version: 8.0.35-0ubuntu0.22.04.1 (Ubuntu)

Copyright (c) 2000, 2023, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql>
```

Si quieres acceder a tu servidor MySQL como root usando un programa externo como phpMyAdmin, tienes dos opciones.

La primera opción es cambiar el método de autenticación de auth_socket a mysql_native_password. Puede hacerlo ejecutando el siguiente comando:

```bash
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'Admcs1234567@';
FLUSH PRIVILEGES;
```

#### Crear el usuairo de acceso remoto

Para una ip especifica

```bash
CREATE USER 'root'@'ip_address' IDENTIFIED WITH caching_sha2_password BY 'xxxxx';
GRANT ALL PRIVILEGES ON *.* TO 'root'@'ip_address';
```

Para cualquier ip

```bash
CREATE USER 'root'@'%' IDENTIFIED WITH caching_sha2_password BY 'Admcs1234567@';
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%';
```

A fin de habilitar un acceso remoto a MySQL es necesario que se habilite la escucha de direcciones IP externas.

```bash
sudo vim /etc/mysql/mysql.conf.d/mysqld.cnf
```

Allí debes ubicar la línea que empieza con la directiva bind-address.

Por defecto, el valor asignado es 127.0.0.1.

Esto significa que el servidor permitirá únicamente conexiones locales.

Tienes que cambiar esta directiva para que haga referencia a una dirección IP externa.

Con fines demostrativos, podemos usar wildcards (comodines) y permitir conexiones remotas en general, sin restringir a direcciones IP específicas.

Para esto asignamos el valor como \*, ::, o 0.0.0.0:

```ini
lc-messages-dir = /usr/share/mysql
skip-external-locking
#
# Instead of skip-networking the default is now to listen only on
# localhost which is more compatible and is not less secure.
bind-address            = 0.0.0.0
```

Luego reinicia el servicio de MySQL para que los cambios realizados en mysqld.cnf tengan efecto:

```bash
sudo systemctl restart mysql
```

Habilitar el puerto de MySQL en el firewall

```bash
sudo ufw allow 3306
```
