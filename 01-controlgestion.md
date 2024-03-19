# Instalación y configuración de Control de Gestión

## **`Backend`** con Laravel 10

**Primero:**

Clona el repositorio en la carpeta de projects

```bash
cd /var/www/html/csdocs/projects
```

**`NAS`** Clona el repositorio en la carpeta web

```bash
cd /volume1/web
```

Clona el repositorio

```bash
git clone https://github.com/CSDocs-dev/ControlGestionBack.git

```

Nos cambiamos de rama sea el caso si no permanecemos en master

```bash
cd ControlGestionBack

git checkout origin develop
```

E**sto creará la carpeta: ControlGestionBack**

1. Prepara la estructura base para composer y php artisan, dentro de la carpeta de la aplicación que acabas de clonar.

```
/var/www/html/csdocs/projects/ControlGestionBack
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
Realiza la configuración del ambiente de trabajo de la aplicación ControlGestionBack.

1. Solicita los archivos KEY de control de gestión.

- Copia los archivos oauth-private.key y oauth-public.key
- Dentro de la carpeta storage de la aplicación:

```
/var/www/html/csdocs/projects/ControlGestionBack/storage
```

1. **`Docker`** Entra al contenedor de MySQL con el usuario root y clave

```bash
docker exec -it mysql bash
```

- Para entrar a la consola de MySQL usa:

```bash
mysql -u root -p
```

- Dentro de la consola de MySQL ejecuta:

```bash
CREATE DATABASE controlgestion;
exit
```

- Para salir del contenedor:

```bash
exit
```

1. Archivo .ENV

```ini
APP_URL=http://controlback.net:50
FRONT_URL='http://localhost:4200'

DB_CONNECTION=mysql
DB_HOST=mysql
DB_PORT=3306
DB_DATABASE=controlgestion
DB_USERNAME=root
DB_PASSWORD=Admcs1234567@

REDIS_HOST=redis_2024
REDIS_PASSWORD=null
REDIS_PORT=6379

# Si es requerido configurar projecto de firma electronica
# ENABLE_CSDOCS_SIGN=1
# CSDOCS_EFIRMA_HOST=http://192.168.2.102:8008
```

1. **`Docker`** Instalación de Laravel, entrar al contenedor de controlgestion ejecutando

```bash
docker exec -it controlgestion_php bash
```

1. Dentro del contenedor ejecuta:

```bash
composer install
```

1. **`NAS`** Si se esta en una NAS:

```bash
/usr/local/bin/php81 /usr/local/bin/composer install
```

1. Valida ejecutando

```bash
php artisan

php artisan --version
```

1. **`NAS`** Si se esta en una NAS:

```bash
/usr/local/bin/php81 artisan
/usr/local/bin/php81 artisan --version
```

1. Ejecución de migraciones y seeders

```bash
php artisan migrate
php artisan migrate:columns
php artisan db:seed
php artisan passport:install
```

1. **`NAS`** Si se esta en una NAS:

```bash
/usr/local/bin/php81 artisan migrate
/usr/local/bin/php81 artisan migrate:columns
/usr/local/bin/php81 artisan db:seed
/usr/local/bin/php81 artisan passport:install
```

1. Crea enlaces simbólicos en storage

```bash
sudo php artisan storage:link
```

1. **`NAS`** Si se esta en una NAS:

```bash
/usr/local/bin/php81 artisan storage:link
```

1. Asignacion de permisos

```bash
sudo chmod -R 777 vendor/mpdf/mpdf/tmp
```

1. **`NAS`** Si se esta en una NAS comentar en public/.htaccess:

```
# Header set Access-Control-Allow-Origin "*"
```

1. Prueba la instalación
   En el explorador web, teclea: [controlback.net:50](http://controlback.net:50)

## `Frontend` con Angular 9

**Primero:**

Clona el repositorio en la carpeta de proyectos frontend

```bash
cd /var/www/html/csdocs/html
```

```bash
git clone https://github.com/CSDocs-dev/controlGestionFront.git
```

Esto creará la carpeta: controlGestionFront

**Segundo:**

Ve a la carpeta del proyecto

```bash
cd controlGestionFront
```

**Tercero:**

Instala las dependencias del proyecto

```bash
sudo npm install --legacy-peer-deps
```

**Cuarto:**

## **Genera la Paleta de Colores**

Pagina Material Design [Palette Generator](http://mcg.mbitson.com/)

Copiar el archivo `src/theme.sample.scss` y renombralo `theme.scss`

```bash
sudo cp src/theme.sample.scss src/theme.scss
```

Insertamos la nueva paleta de color generada `src/theme.scss`

```scss
$mat-custom: (
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
  contrast: (
    50: #000000,
    100: #000000,
    200: #000000,
    300: #ffffff,
    400: #ffffff,
    500: #ffffff,
    600: #ffffff,
    700: #ffffff,
    800: #ffffff,
    900: #ffffff,
    A100: #000000,
    A200: #000000,
    A400: #ffffff,
    A700: #ffffff,
  ),
);

$custom-theme-primary: mat-palette($mat-custom, 500);
$custom-theme-accent: mat-palette($mat-custom, 700, 100, 400);
$custom-theme-warn: mat-palette($mat-custom, 600);
```

**Crear carpeta de environments dentro de src**

```bash
sudo mkdir -m 777 environments
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
  apiUrl: "http://localhost:50",
  pdfjsAssetsPath: "/assets/pdfjs/",
};
```

**Archivo environment.prod.ts**

```ts
export const environment = {
  production: true,
  apiUrl: "http://localhost:50",
  pdfjsAssetsPath: "/assets/pdfjs/",
};
```

Ejecución de proyecto editor

```bash
cd projects/editor/src/ckeditor5-build-decoupled-document

sudo npm install --legacy-peer-deps

sudo npm run build
```

**Creamos environment de editor**

```bash
sudo vim projects/editor/src/environments/environment.ts
```

```ts
export const environment = {
  production: false,
  apiUrl: "http://localhost:50",
  pdfjsAssetsPath: "/assets/pdfjs/",
};
```

**Creamos environment de archivistica**

```bash
sudo vim projects/archivistica/src/environments/environment.ts
```

```ts
export const environment = {
  production: false,
  apiUrlCG: "http://localhost:50",
};
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
