# Instalacion y Configuracion Supervisor

Para instalar Supervisor en Ubuntu 22.04, puedes seguir estos pasos:

1. Actualiza el índice de paquetes:

   ```bash
   sudo apt update
   ```

2. Instala Supervisor:

   ```bash
   sudo apt install supervisor
   ```

3. Una vez instalado, puedes verificar el estado de Supervisor:

   ```bash
   sudo systemctl status supervisor
   ```

4. Para asegurarte de que Supervisor se inicie automáticamente al arrancar el sistema, ejecuta:

   ```bash
   sudo systemctl enable supervisor
   ```

5. Ahora puedes empezar a utilizar Supervisor para administrar tus procesos. Puedes crear archivos de configuración para tus procesos en el directorio `/etc/supervisor/conf.d/`. Cada archivo de configuración corresponderá a un proceso que Supervisor deberá administrar.

6. Después de crear o modificar archivos de configuración, recarga Supervisor para que tome los cambios:

   ```bash
   sudo systemctl reload supervisor
   ```

7. Para comenzar un proceso específico, puedes utilizar el comando `supervisorctl start nombre_del_proceso`. Por ejemplo:

   ```bash
   sudo supervisorctl start myapp
   ```

Con estos pasos, deberías tener Supervisor instalado y listo para administrar tus procesos en Ubuntu 22.04.

### Crear una nueva tarea

Para crear una nueva tarea (o proceso) que Supervisor pueda administrar, sigue estos pasos:

1. Crea un archivo de configuración para tu tarea en el directorio `/etc/supervisor/conf.d/`. Puedes utilizar cualquier nombre para el archivo, pero es común usar la extensión `.conf`. Por ejemplo, crea un archivo llamado `controlgestion.conf`:

   ```bash
   sudo vim /etc/supervisor/conf.d/controlgestion.conf
   ```

2. Dentro de este archivo, define la configuración de tu tarea.

   ```ini
   [supervisord]
   logfile=/etc/supervisor/logs/supervisord.log ; main log file; default $CWD/supervisord.log
   logfile_maxbytes=5MB         ; max main logfile bytes b4 rotation; default 50MB
   logfile_backups=10           ; # of main logfile backups; 0 means none, default 10
   loglevel=info                ; log level; default info; others: debug,warn,trace
   pidfile=/tmp/supervisord.pid ; supervisord pidfile; default supervisord.pid
   nodaemon=false               ; start in foreground if true; default false
   minfds=1024                  ; min. avail startup file descriptors; default 1024
   minprocs=200                 ; min. avail process descriptors;default 200

   [rpcinterface:supervisor]
   supervisor.rpcinterface_factory = supervisor.rpcinterface:make_main_rpcinterface

   # ControlGestion
   [program:laravel-worker-EmailNotification]
   command=php /var/www/html/cg/artisan queue:work redis --queue=EmailNotification --sleep=3 --tries=1
   directory=/var/www/html/cg
   autostart=true
   autorestart=true
   redirect_stderr=true
   stdout_logfile=/var/www/html/cg/storage/logs/worker_EmailNotification.log

   [program:laravel-worker-ClasificacionArchivistica]
   command=php /var/www/html/cg/artisan queue:work redis --queue=ClasificacionArchivistica --sleep=3 --tries=1
   directory=/var/www/html/cg
   autostart=true
   autorestart=true
   redirect_stderr=true
   stdout_logfile=/var/www/html/cg/storage/logs/worker_ClasificacionArchivistica.log
   ```

3. Guarda el archivo y cierra el editor.

4. Recarga la configuración de Supervisor para que tome los cambios:

```bash
sudo supervisorctl reread
sudo supervisorctl update
```

5. Ahora puedes empezar tu tarea con el siguiente comando:

   ```bash
   sudo supervisorctl start controlgestion
   ```

   Sustituye `controlgestion` por el nombre que has utilizado en el archivo de configuración.

Con estos pasos, has creado una nueva tarea que Supervisor puede administrar. Puedes repetir este proceso para agregar más tareas según tus necesidades.
