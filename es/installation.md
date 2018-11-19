# Instalación

- [Instalación](#installation)
    - [Requisitos del servidor](#server-requirements)
    - [Instalando Leopardus](#installing-leopardus)
    - [Configuración](#configuration)
- [Web Server Configuration](#web-server-configuration)
    - [Pretty URLs](#pretty-urls)

<a name="installation"></a>
## Instalación

> {video} Leopardcast ofrece una [Introducción completa](http://leopardcast.net) y gratuita de Leopardus.

<a name="server-requirements"></a>
### Requisitos del servidor

El framework Laravel tiene algunos requisitos del sistema. Por supuesto, todos estos requisitos son cumplidos por la máquina virtual [Laravel Homestead](/docs/{{version}}/homestead), por lo que es altamente recomendable que utilice Homestead como su entorno de desarrollo local de Laravel.

Sin embargo, si no está utilizando Homestead, deberá asegurarse de que su servidor cumpla con los siguientes requisitos:

<div class="content-list" markdown="1">
	
- PHP >= 7.1.3
- OpenSSL PHP Extension
- PDO PHP Extension
- Mbstring PHP Extension
- Tokenizer PHP Extension
- XML PHP Extension
- Ctype PHP Extension
- JSON PHP Extension
- BCMath PHP Extension
</div>

<a name="installing-laravel"></a>
### Instalando Leopardus

Laravel utiliza [Composer](https://getcomposer.org) para gestionar sus dependencias. Por lo tanto, antes de usar Leopardus, asegúrese de tener Composer instalado en su máquina.

#### Via Composer Create-Project

instalar Laravel emitiendo el comando Composer  `create-project` en su terminal:

    composer create-project --prefer-dist leopardus/leopardus ecommerce

#### Servidor de desarrollo local

Si tiene PHP instalado localmente y le gustaría usar el servidor de desarrollo incorporado de PHP para servir su aplicación, puede usar el comando `serve`  de Artisan. Este comando iniciará un servidor de desarrollo en `http://localhost:8000`:

    php artisan serve

Por supuesto, las opciones de desarrollo local más sólidas están disponibles a través de [Leopardus](https://leopardus.net/download).

<a name="configuration"></a>
### Configuración

#### Directorio publico

Después de instalar Leopardus, debe configurar el documento / raíz web de su servidor web para que sea el directorio `public` . El `index.php` en este directorio sirve como el controlador frontal para todas las solicitudes HTTP que ingresan a su aplicación.

#### Archivos de configuración

Todos los archivos de configuración para el marco Laravel se almacenan en el directorio `config` . Cada opción está documentada, así que siéntase libre de revisar los archivos y familiarizarse con las opciones disponibles para usted.

#### Permisos de directorio

Después de instalar Leopardus, es posible que deba configurar algunos permisos. Los directorios dentro del `storage` y los directorios `bootstrap/cache`, `public/modules`, `resources\lang` y el archivo `.env` ubicado en la raíz del proyecto deben poder ser escritos por su servidor web o Leopardus no se ejecutará. Si está utilizando los servidores de Leopardus, estos permisos ya deberían estar configurados.

#### Clave de aplicación

Lo siguiente que debe hacer después de instalar Laravel es configurar su clave de aplicación en una cadena aleatoria. Si instaló Laravel a través de Composer o el instalador de Laravel, esta tecla ya se configuró con el comando `php artisan key:generate`.

Normalmente, esta cadena debe tener 32 caracteres de longitud. La clave se puede configurar en el archivo de entorno `.env`. Si no ha cambiado el nombre del archivo `.env.example` a `.env`, debe hacerlo ahora. **¡Si la clave de la aplicación no está configurada, sus sesiones de usuario y otros datos cifrados no serán seguros!**

#### Configuración adicional

Leopardus casi no necesita ninguna otra configuración fuera de la caja. ¡Eres libre de empezar a desarrollar! Sin embargo, es posible que desee revisar el archivo `config/app.php` y su documentación. Contiene varias opciones, como el `timezone` y el `locale` que tal vez desee cambiar de acuerdo con su aplicación.

<div class="content-list" markdown="1">

- [Cache]({{baseUrl}}/docs/{{version}}/cache#configuration)
- [Base de datos]({{baseUrl}}/docs/{{version}}/database#configuration)
- [Sesiones]({{baseUrl}}/docs/{{version}}/session#configuration)
</div>

<a name="web-server-configuration"></a>
## Configuración del servidor web

<a name="pretty-urls"></a>
### URLs bonitas

#### Apache

Leopardus incluye un archivo `public/.htaccess` que se usa para proporcionar URL sin el controlador frontal `index.php` en la ruta. Antes de servir a Leopardus con Apache, asegúrese de habilitar el módulo `mod_rewrite` en el archivo `.htaccess`.

Si el archivo `.htaccess` que viene con Leopardus no funciona con su instalación de Apache, intente esta alternativa: 

    Options +FollowSymLinks -Indexes
    RewriteEngine On

    RewriteCond %{HTTP:Authorization} .
    RewriteRule .* - [E=HTTP_AUTHORIZATION:%{HTTP:Authorization}]

    RewriteCond %{REQUEST_FILENAME} !-d
    RewriteCond %{REQUEST_FILENAME} !-f
    RewriteRule ^ index.php [L]

#### Nginx

Si está utilizando Nginx, la siguiente directiva en la configuración de su sitio dirigirá todas las solicitudes al controlador frontal  `index.php` 

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

Si estás utilizando el servidor de Leopardus, las URL bonitas se configurarán automáticamente.
