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
3. Configuracion de Datos de Base de datos y demas plataformas en "app.yaml":
4. Aplicar rutas de librerias en "routes.php":
```php
    /** AUTHENTICATION **/
    $app->route('/mia-auth/login', [Mia\Auth\Handler\LoginHandler::class], ['GET', 'POST', 'OPTIONS', 'HEAD'], 'mia_auth.login');
    $app->route('/mia-auth/register', [\Mia\Mail\Handler\SendgridHandler::class, new RegisterHandler(false)], ['GET', 'POST', 'OPTIONS', 'HEAD'], 'mia_auth.register');
    $app->route('/mia-auth/update-profile', [Mia\Auth\Handler\UpdateProfileHandler::class], ['GET', 'POST', 'OPTIONS', 'HEAD'], 'mia_auth.update-profile');
    $app->route('/mia-auth/recovery', [\Mia\Mail\Handler\SendgridHandler::class, Mia\Auth\Handler\MiaRecoveryHandler::class], ['GET', 'POST', 'OPTIONS', 'HEAD'], 'mia_auth.recovery');
    $app->route('/mia-auth/change-password-recovery', [Mia\Auth\Handler\MiaPasswordRecoveryHandler::class], ['GET', 'POST', 'OPTIONS', 'HEAD'], 'mia_auth.change-password-recovery');
    $app->route('/mia-auth/me', [\Mia\Auth\Handler\AuthInternalHandler::class, Mia\Auth\Handler\FetchProfileHandler::class], ['GET', 'POST', 'OPTIONS', 'HEAD'], 'mia_auth.me');
    
    //$app->route('/mia-auth/google-signin', [Mia\Auth\Handler\GoogleSignInHandler::class], ['GET', 'POST', 'OPTIONS', 'HEAD'], 'mia_auth.google-signin');
    //$app->route('/mia-auth/apple-signin', [Mia\Auth\Handler\AppleSignInHandler::class], ['GET', 'POST', 'OPTIONS', 'HEAD'], 'mia_auth.apple-signin');
    $app->route('/mia-auth/register-device', [\Mia\Auth\Handler\AuthInternalHandler::class, Mia\Auth\Handler\RegisterDeviceHandler::class], ['GET', 'POST', 'OPTIONS', 'HEAD'], 'mia_auth.register-device');

    //$app->route('/mia-auth/role/list', [Mia\Auth\Handler\Role\ListHandler::class], ['GET', 'POST', 'OPTIONS', 'HEAD'], 'mia_auth.role-list');

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