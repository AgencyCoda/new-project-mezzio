# Creacion de proyecto para APIs
1. Crear proyecto
```bash
composer create-project mezzio/mezzio-skeleton name_of_project
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

## Como iniciar servidor para testear
```bash
composer run --timeout=0 serve
```

## Como crear un modulo y subirlo a composer
1. Crear repositorio en Github dentro de la organización: AgencyCoda
MUY IMPORTANTE: El nombre del paquete tiene que empezar con "mia-" y terminar con "-mezzio", Ejemplo: "mia-pushbullet-mezzio"

![Imagen](https://raw.githubusercontent.com/AgencyCoda/new-project-mezzio/main/images/modulo_1.png)