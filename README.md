# Creacion de proyecto para APIs
1. Crear proyecto
```bash
composer create-project agencycoda/mezzio-skeleton name_of_project
```
2. Activar configuración personalizada:
- Modular
- Laminas Service Manager
- Fast Route
- None template
- Whoops
3. Agregar repositorios en el composer.json:
```js
"repositories": [
        {
            "type": "git",
            "url": "https://github.com/agencycoda/mia-eloquent-mezzio.git"
        },
        {
            "type": "git",
            "url": "https://github.com/agencycoda/mia-core-mezzio.git"
        },
        {
            "type": "git",
            "url": "https://github.com/agencycoda/mia-auth-mezzio.git"
        }
    ]
```
4. Configuración de la base de datos:
```bash
composer require agencycoda/mia-core-mezzio
composer require agencycoda/mia-eloquent-mezzio
composer require agencycoda/mia-auth-mezzio
composer require mezzio/mezzio-cors
```
5. Abrir public/index.php
```php
/** @var \Psr\Container\ContainerInterface $container */
$container = require 'config/container.php';

// Inicializar Database
Mia\Database\Eloquent::install($container);
```
6. Crear archivo "eloquent.global.php" en "config/autoload":
```php
return [
    'eloquent' => [
        'driver'    => 'mysql',
        'host'      => 'localhost:8889',
        'database'  => 'expressive_test',
        'username'  => 'root',
        'password'  => 'root',
        'charset'   => 'utf8',
        'collation' => 'utf8_general_ci',
    ]
];
```

## Aplicar Rutas de librerias
```php
    /** EMAILs Templates  */
    $app->route('/mia-mail-admin/list', [\Mia\Mail\Handler\FetchTemplatesHandler::class], ['GET', 'POST', 'OPTIONS', 'HEAD'], 'mia-mail.list');
    $app->route('/mia-mail-admin/save', [\Mia\Mail\Handler\SaveTemplateHandler::class], ['GET', 'POST', 'OPTIONS', 'HEAD'], 'mia-mail.save');
```

## Como iniciar servidor para testear
```bash
composer run --timeout=0 serve
```

## Generar documentación con Swagger
```bash
./vendor/bin/openapi --format json --output public/swagger/swagger.json src config vendor/agencycoda
```

## Como crear un modulo y subirlo a composer
1. Crear repositorio en Github dentro de la organización: "AgencyCoda"

MUY IMPORTANTE: El nombre del paquete tiene que empezar con "mia-" y terminar con "-mezzio", Ejemplo: "mia-pushbullet-mezzio"

![Imagen](https://raw.githubusercontent.com/AgencyCoda/new-project-mezzio/main/images/modulo_1.png)

2. Clonar repositorio:
```bash
git clone https://github.com/AgencyCoda/mia-pushbullet-mezzio.git
```

3. Crear archivo composer.json, Estructura de ejemplo:
```json
{
    "name": "agencycoda/mia-pushbullet-mezzio",
    "description": "The library for Mezzio",
    "version": "0.0.1",
    "minimum-stability": "stable",
    "license": "MIT",
    "authors": [{
        "name": "Matias Camiletti",
        "homepage": "https://agencycoda.com"
    }],
    "require": {
        "php": "^7.3"
    },
    "autoload": {
        "psr-4": {
            "Mia\\PushBullet\\": "src/"
        }
    },
    "scripts": {
        "serve": "php -S 0.0.0.0:8080 -t example/"
    }
}
```

Reemplazar name y autoload

4. Crear carpeta "src".
5. Dentro de la carpeta src, subir/agregar los archivos de la libreria. Ejemplo de estructura:
```php
- src/
    - PushBulletHelper.php
    - Handler/
        - PushHandler.php
```

6. MUY IMPORTANTE: no agregar en estas librerias datos de acceso, tokens, contraseñas, etc.

7. Si se quiere agregar codigo de ejemplo, pruebas, se puede crear la carpeta "example"