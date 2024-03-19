# Instalación y configuración de Content

## **`Backend`** con Laravel 8

**Primero:**

Clona el repositorio en la carpeta de projects

```bash
git clone https://Osvaldo-ACR@bitbucket.org/csdocs/content-light-back.git
```

Nos cambiamos de rama sea el caso si no permanecemos en master

```bash
git checkout origin develop
```

Esto creará la carpeta: content-light-back

1. Prepara la estructura base para composer y php artisan, dentro de la carpeta de la aplicación que acabas de clonar.

```
/var/www/html/csdocs/projects/content-light-back
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
Realiza la configuración del ambiente de trabajo de la aplicación contentLightBack.

1. Solicita los archivos KEY de control de gestión.

- Copia los archivos oauth-private.key y oauth-public.key
- Dentro de la carpeta storage de la aplicación:

```
/var/www/html/csdocs/projects/content-light-back/storage
```

1. Archivo .ENV

```ini
APP_URL=http://contentback.net:70
FRONT_URL='http://localhost:4200'

DB_CONNECTION=mysql
DB_HOST=mysql
DB_PORT=3306
DB_DATABASE=csdocs
DB_USERNAME=root
DB_PASSWORD=Admcs1234567@

REDIS_HOST=redis
REDIS_PASSWORD=null
REDIS_PORT=6379

# Configura ruta de carpetas auxiliares
```

**Asignación de permisos a carpetas auxiliares ubicadas en** /var/www/html/csdocs/folders

```bash
sudo chmod -R 777 TEMPDIR
sudo chmod -R 777 error
sudo chmod -R 777 gestor_carga
sudo chmod -R 777 trash_publisher
sudo chmod -R 777 backup
sudo chmod -R 777 gestor
sudo chmod -R 777 publisher

```

1. Instalación de Laravel, entra al contenedor de content ejecutando

```bash
docker exec -it content_php bash
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

1. Creacion de base de datos core

```bash
php artisan create:csdocs
```

1. Crea enlaces simbólicos en storage

```bash
php artisan storage:link
```

1. Prueba la instalación
   En el explorador web, teclea: [contentback.net:70](http://contentback.net:70)

## **`Frontend`**

## `Frontend` con Angular 14

**Primero:**

Clona el repositorio en la carpeta de proyectos

```bash
cd /var/www/html/csdocs/html
```

```bash
git clone https://Osvaldo-ACR@bitbucket.org/csdocs/content-light-front.git

```

Esto creará la carpeta: content-light-front

**Segundo:**

Ve a la carpeta del proyecto

```bash
cd content-light-front

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

```
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
  apiUrl: "http://localhost:70",
  pdfjsAssetsPath: "/assets/pdfjs/",
  typeQR: 2,
};
```

**Archivo environment.prod.ts**

```ts
export const environment = {
  production: true,
  apiUrl: "http://localhost:70",
  pdfjsAssetsPath: "/assets/pdfjs/",
  typeQR: 2,
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
