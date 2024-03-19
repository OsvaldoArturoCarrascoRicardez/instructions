# Instalación y configuración de Firma

## **`Backend`** con Laravel 7

**Primero:**

Clona el repositorio en la carpeta de projects

```bash
git clone https://Osvaldo-ACR@bitbucket.org/csdocs/csdocs-efirma.git
```

E**sto creará la carpeta: csdocs-efirma**

1. Prepara la estructura base para composer y php artisan, dentro de la carpeta de la aplicación que acabas de clonar.

```
/var/www/html/csdocs/projects/csdocs-efirma
├── bootstrap
│   └── cache
└── storage
		└── app
		└── logs
    └── framework
        ├── sessions
        ├── views
        └── cache
				└── testing
```

**Creación de carpetas**

```bash
sudo mkdir -m 777 storage
sudo mkdir -m 777 storage/framework
sudo mkdir -m 777 storage/framework/views
sudo mkdir -m 777 storage/framework/cache
sudo mkdir -m 777 storage/framework/sessions
sudo mkdir -m 777 storage/framework/testing
sudo mkdir -m 777 storage/logs
sudo mkdir -m 777 storage/app
sudo mkdir -m 777 bootstrap/cache
sudo mkdir -m 777 storage/efirma
sudo mkdir -m 777 storage/efirma/SystemSetting
```

**Asignación de permisos**

```bash
sudo chmod 777 bootstrap
sudo chmod 777 bootstrap/cache
sudo chmod 777 storage/app
sudo chmod 777 storage/framework
sudo chmod 777 storage/framework/views
sudo chmod 777 storage/framework/cache
sudo chmod 777 storage/framework/cache/data
sudo chmod 777 storage/framework/sessions
sudo chmod 777 storage/framework/testing
sudo chmod 777 storage/logs
sudo chmod -R 777 storage/efirma
sudo chmod -R 777 storage/efirma/SystemSetting
```

**Creación de archivo de log**

```bash
sudo vim storage/logs/laravel.log

sudo chmod 777 storage/logs/laravel.log
```

**Segundo:**

- Asignar permisos para abrir el archivo signstamp.ini
- Asignar permisos a la carpeta storage/efirma/SystemSetting

**Ejecutar via api (consultar json.postam)**

http://localhost:85/api/system/upload/signstamp

http://localhost:85/api/system/upload/template

http://localhost:85/api/system/upload/image

1. Archivo .ENV

```ini
APP_URL=http://efirmaback:90

## Base de Datos local de e-firma
DB_CONNECTION=mysql
DB_HOST=mysql
DB_PORT=3306
DB_DATABASE=csdocs_sign
DB_USERNAME=root
DB_PASSWORD=Admcs1234567@

## Datos de conexión para Control de Gestión
CG_DB_CONNECTION=mysql
CG_DB_HOST=mysql
CG_DB_PORT=3306
CG_DB_DATABASE=controlgestion
CG_DB_USERNAME=root
CG_DB_PASSWORD=Admcs1234567@
```

1. Instalación de Laravel, entra al contenedor de firma ejecutando

```bash
docker exec -it firma_php bash
```

1. Dentro del contenedor ejecuta:

```bash
composer install
```

1. Valida ejecutando

```bash
php artisan

php artisan --version
```

1. Ejecución de base de datos core

```bash
php artisan csdocs:default
```

1. Crea enlaces simbólicos en storage

```bash
php artisan storage:link
```

1. Prueba la instalación
   En el explorador web, teclea: [firmaback.net:90](http://firmaback.net:90)

**Fin**
