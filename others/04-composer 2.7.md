# Instalacion y Configuracion Composer

### **`Composer`**

Para instalar Composer en tu servidor Ubuntu, sigue estos pasos:

1. **Descargar e instalar Composer**:

   Ejecuta los siguientes comandos para descargar e instalar Composer:

```bash
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
php -r "if (hash_file('sha384', 'composer-setup.php') === 'dac665fdc30fdd8ec78b38b9800061b4150413ff2e3b6f88543c636f7cd84f6db9189d43a81e5503cda447da73c7e5b6') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
php composer-setup.php
php -r "unlink('composer-setup.php');"
```

Estos comandos descargarán el instalador de Composer, verificarán su integridad y luego lo instalarán en tu sistema.

2. **Mover el archivo Composer al directorio global**:

   Para que Composer esté disponible globalmente en tu sistema, muévelo al directorio `/usr/local/bin`:

```bash
sudo mv composer.phar /usr/local/bin/composer
```

Esto permite que puedas ejecutar el comando `composer` desde cualquier ubicación en tu sistema.

3. **Verificar la instalación de Composer**:

   Para verificar que Composer se haya instalado correctamente, puedes ejecutar:

```bash
composer --version
```

Esto mostrará la versión de Composer instalada en tu sistema.

Con estos pasos, deberías tener Composer instalado y listo para usar en tu servidor Ubuntu. Puedes usar Composer para gestionar las dependencias de tus proyectos PHP.
