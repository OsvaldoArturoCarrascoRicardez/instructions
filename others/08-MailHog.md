# Instalación y configuración de MailHog

## Instalación y configuración de MailHog en Ubuntu

**Primero:**

Instala Go en tu sistema utilizando el gestor de paquetes apt.

```
sudo apt update
sudo apt install golang-go
```

Comprueba la instalación de Go con:

```
go version
```

**Segundo:**

Descarga e instala MailHog.

```
go get github.com/mailhog/MailHog
```

**Tercero:**

Ahora, necesitas hacer que MailHog se ejecute en el inicio. Crea un nuevo servicio systemd.

**Tercero:**

Ahora, necesitas hacer que MailHog se ejecute en el inicio. En lugar de utilizar systemd, vamos a utilizar Supervisor para este propósito. Una vez instalado, necesitas crear un nuevo archivo de configuración para tu servicio MailHog. Puedes hacerlo utilizando el siguiente comando:

```
sudo nano /etc/supervisor/conf.d/mailhog.conf
```

Agrega el siguiente contenido al archivo. (sustituir por ruta donde se instalo MailHog)

```
[program:mailhog]
command=/home/USERNAME/go/bin/MailHog
autostart=true
autorestart=true
user=USERNAME
```

**Quinto:**

Actualiza Supervisor y haz que inicie MailHog.

```
sudo supervisorctl reread
sudo supervisorctl update
sudo supervisorctl start mailhog
```

**Sexto:**

Finalmente, configura tu aplicación para usar MailHog como servidor SMTP. Generalmente, esto implica establecer el servidor SMTP a localhost y el puerto a 1025.
