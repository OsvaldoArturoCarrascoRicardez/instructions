# Instalación y configuración de Archivistica

## **`Backend`** con Laravel 7

**Primero:**

Clona el repositorio en la carpeta de projects

```bash
git clone https://Osvaldo-ACR@bitbucket.org/csdocs/archivisticaback.git
```

Nos cambiamos de rama sea el caso si no permanecemos en master

```bash
git checkout origin develop
```

**Esto creará la carpeta: archivisticaback**

1. Prepara la estructura base para composer y php artisan, dentro de la carpeta de la aplicación que acabas de clonar.

```
/var/www/html/csdocs/projects/archivisticaback
├── bootstrap
│   └── cache
└── storage
		└── app
		└── logs
    └── framework
        ├── sessions
        ├── views
        └── cache
			      └── data
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
```

**Creación de archivo de log**

```bash
sudo vim storage/logs/laravel.log

sudo chmod 777 storage/logs/laravel.log
```

**Segundo:**
Realiza la configuración del ambiente de trabajo de la aplicación archivisticaback.

1. Solicita los mismos archivos KEY de control de gestión.

- Copia los archivos oauth-private.key y oauth-public.key
- Dentro de la carpeta storage de la aplicación:

```
/var/www/html/csdocs/projects/archivisticaback/storage
```

1. Archivo .ENV

```ini
APP_URL=http://archivisticaback.net:60
FRONT_URL='http://localhost:4200'

#Configurar instamcia cs-docs, esto únicamente en caso de que el cliente ya tenga una instancia configurada con MySQL
CSDOCS_DB_CONNECTION=mysql
CSDOCS_DB_HOST=mysql
CSDOCS_DB_PORT=3306
CSDOCS_DB_DATABASE=cs-docs
CSDOCS_DB_USERNAME=root
CSDOCS_DB_PASSWORD=Admcs1234567@

#Esta conexión es la conexión que debe de ir a la única instancia existente
DB_CONNECTION=mongodb
DB_HOST=mysql
DB_PORT=3306
DB_DATABASE=archivistica
DB_USERNAME=root
DB_PASSWORD=Admcs1234567@

#Conexión a control de gestión
CG_DB_CONNECTION=mysql
CG_DB_HOST=mysql
CG_DB_PORT=3306
CG_DB_DATABASE=controlgestion
CG_DB_USERNAME=root
CG_DB_PASSWORD=Admcs1234567@

#Conexión a MongoDB
MONGO_DB_HOST=mongodb
MONGO_DB_PORT=27017
MONGO_DB_DATABASE=archivistica
MONGO_DB_USERNAME=root
MONGO_DB_PASSWORD=secret

REDIS_HOST=redis
REDIS_PASSWORD=null
REDIS_PORT=6379

# Configurar el resto de rutas necesarias
```

1. Instalación de Laravel, entra al contenedor de controlgestion ejecutando

```bash
docker exec -it archivistica_php bash
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

1. Ejecución de migraciones y seeders

```bash
php artisan migrate
php artisan migrate:columns
php artisan db:seed
```

1. Crea enlaces simbólicos en storage

```bash
sudo php artisan storage:link
```

1. Prueba la instalación
   En el explorador web, teclea: [archivisticaback.net:60](http://archivisticaback.net:60)

## `Frontend` con Angular 14

**Primero:**

Clona el repositorio en la carpeta de proyectos frontend

```bash
cd /var/www/html/csdocs/html
```

```bash
git clone https://Osvaldo-ACR@bitbucket.org/csdocs/archivisticafrontv2.git
```

Esto creará la carpeta: archivisticafrontv2

**Segundo:**

Ve a la carpeta del proyecto

```bash
cd archivisticafrontv2
```

**Tercero:**

Instala las dependencias del proyecto

```bash
sudo npm install --legacy-peer-deps
```

**Cuarto:**

## **Genera la Paleta de Colores**

Pagina Material Design [Palette Generator](http://mcg.mbitson.com/)

Copiar el archivo `src/assets/scss/_material.example.scss` y renombrarlo `src/assets/scss/_material.scss`

```bash
sudo cp src/assets/scss/_material.example.scss src/assets/scss/_material.scss
```

Insertamos la nueva paleta de color generada `src/assets/scss/_material.scss`

```scss
$archivist-primary: (
  50: #efe6e9,
  100: #d7bfc8,
  200: #bd95a3,
  300: #a26b7e,
  400: #8e4b62,
  500: #7a2b46,
  600: #72263f,
  700: #672037,
  800: #5d1a2f,
  900: #4a1020,
  A100: #ff84a1,
  A200: #ff517a,
  A400: #ff1e53,
  A700: #ff043f,
  contrast: $contrast,
);
```

**Estructura de la Carpeta Environments**

```bash
src/
├── app/
├── assets/
├── environments/
│   ├── environment.prod.ts
│   └── environment.ts
└── ...

```

**Archivo environment.ts**

```ts
export const environment = {
  production: false,
  apiUrl: "http://localhost:60",
  apiUrlCG: "http://localhost:50",
  pdfjsAssetsPath: "/assets/pdfjs/",
};
```

**Archivo environment.prod.ts**

```ts
export const environment = {
  production: true,
  apiUrl: "http://localhost:60",
  apiUrlCG: "http://localhost:50",
  pdfjsAssetsPath: "/assets/pdfjs/",
};
```

**Cuarto:**

Instala las dependencias del proyecto CKEditor

```bash
cd src/assets/ckeditor5-build-decoupled-document

sudo npm install --legacy-peer-deps

sudo npm run build

cd ../../../

```

Inicia la aplicación en modo desarrollo si se requiere

```bash
ng serve
```

Una vez que el servidor está arriba, puedes abrir el navegador y visitar `http://localhost:4200` para ver tu aplicación.

**Nota:** Si quieres que la aplicación se ejecute en un puerto diferente, puedes usar:

```bash
ng serve --port=4201
```

**Quinto:**

Para generar una versión de producción de tu aplicación, puedes usar:

```bash
sudo ng build
```

Esto creará una carpeta dist en el directorio raíz de tu proyecto. Dentro de esta carpeta, encontrarás todos los archivos necesarios para desplegar la aplicación en el servidor.

## **Extra (Crear Tarea en Supervisor para Correos):**

Ejecutar para crear tarea:

```bash
sudo vim /etc/supervisor/conf.d/archivistica.conf
```

Declaramos la tarea:

```ini
# Archivistica
[program:emailNotification-Archival]
command=php /var/www/html/dev/arch/archivisticaback/artisan queue:work redis --queue=EmailNotification --sleep=3 --tries=1
directory=/var/www/html/dev/arch/archivisticaback
autostart=true
autorestart=true
redirect_stderr=true
stdout_logfile=/var/www/html/dev/arch/archivisticaback/storage/logs/worker_EmailNotification.log
```

Actualizamos la configuracion de supervisor:

```bash
sudo supervisorctl reread

sudo supervisorctl update
```

Verificamos que ya se encuentre en el listado de tareas:

```bash
sudo supervisorctl status
```
