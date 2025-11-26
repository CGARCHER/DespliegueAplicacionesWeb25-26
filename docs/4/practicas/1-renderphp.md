# Despliegue de Aplicación MVC PHP en Render

Esta práctica explica cómo desplegar una aplicación PHP (patrón MVC) en Render y conectarla a una base de datos en la nube (SkySQL). El objetivo es que el mismo código funcione en local y en producción sin cambios, usando variables de entorno y una configuración flexible de la conexión a la base de datos.

Qué aprenderás:
- Separar entornos (local vs producción).
- Gestionar credenciales con variables de entorno.
- Configurar Render y SkySQL.
- Mantener código portátil y fácil de mover entre entornos.

---

## Variables de entorno

Por seguridad no se deben subir credenciales al repositorio. En Render configura al menos estas variables:

```
DB_HOST=tu-host-de-base-de-datos
DB_NAME=nombre-de-tu-base-de-datos
DB_USER=usuario-de-base-de-datos
DB_PASSWORD=contraseña-de-base-de-datos
DB_PORT=3306
APP_ENV=production
BASE_URL=https://tu-servicio.onrender.com
```

---

## Pasos para el despliegue

1) Crear la base de datos en SkySQL
    - Registrarse en SkySQL y crear un servicio MariaDB/MySQL.
    - Guardar host, puerto, usuario, contraseña y nombre de la base de datos.

2) Preparar la configuración de la aplicación
    - No incluir credenciales en el código; usar `getenv()` para leer las variables.
    - En el arranque o en la clase de configuración detectar si `DB_HOST` está definida. Si existe, montar el DSN desde las variables; si no, cargar los parámetros desde `config.ini` para uso local. Por ejemplo:

```php
<?php
// Si estamos en Render usamos variables de entorno
if (getenv('DB_HOST')) {

    $host = getenv('DB_HOST');
    $name = getenv('DB_NAME');
    $user = getenv('DB_USER');
    $pass = getenv('DB_PASSWORD');
    $port = getenv('DB_PORT') ?: 3306;

    // construimos el DSN igual que en config.ini
    self::$host = "mysql:host={$host};port={$port};dbname={$name};charset=utf8mb4";
    self::$user = $user;
    self::$pass = $pass;

} else {
    // En local, leer config.ini como siempre
    $conf = parse_ini_file('config.ini');
    self::$host = $conf['host'];
    self::$user = $conf['user'];
    self::$pass = $conf['pass'];
}
?>
```

- Instanciar la clase de conexión incluida en `config/PDO.php` con el DSN, usuario y contraseña preparados. Adaptar la llamada al constructor a la firma real del archivo.
- Definir la URL base en `config.php` priorizando `getenv('BASE_URL')` y, si no existe, usando `base_url` desde `config.ini`. Ajustar la ruta de `parse_ini_file()` según el fichero que ejecute la configuración.

3) Crear el servicio en Render
    - Conectar el repositorio de GitHub.
    - Seleccionar la rama a desplegar (por ejemplo `deploy_render`).
    - Configurar runtime/instancia (Docker u otro) y crear el servicio.

4) Configurar variables en el panel de Render
    - Añadir las variables `DB_*` y `BASE_URL`.
    - Guardar; Render redeplegará la aplicación con la nueva configuración.

5) Verificar el despliegue
    - Acceder a la URL pública que proporciona Render.
    - Probar las funcionalidades que usan la base de datos.
    - Consultar los logs de Render si aparecen errores.

---

## Problema con PDO en Render

### El problema (qué suele aparecer)

Al desplegar en Render puede aparecer un error de conexión o permisos como:

```
SQLSTATE[HY000] [1045] Access denied for user 'dbpgf05703062'@'79.116.122.22' (using password: YES)
```

- Host, usuario o contraseña incorrectos en las variables de entorno.
- El usuario de la base de datos no tiene permisos para conectarse desde la IP del servicio.
- Firewall o reglas de red en SkySQL bloqueando la conexión.
- Timeouts o parámetros del driver incompatibles con el entorno de producción.

Detectado el error, revisar las variables en el panel de Render y los logs para identificar la causa exacta.

### La solución (qué hacer después de detectar el error)

- Confirmar que las variables `DB_*` en Render coinciden con las credenciales de SkySQL.
- Autorizar la IP o el rango desde el que Render conecta en la configuración de SkySQL.
- Usar la clase de conexión incluida en `config/PDO.php` (wrapper sobre PDO) en lugar de instanciar PDO sin ajustes; el wrapper centraliza atributos y timeouts adecuados para producción.
- Añade esta clase a tu proyecto en la carpeta config -> https://github.com/CGARCHER/MVC-PHP-Producto-Docker/blob/deploy_render/config/PDO.php
- Si el problema persiste, revisar logs tanto en SkySQL como en Render para obtener más detalles.

No es necesario modificar el wrapper para esta práctica; instanciar la clase existente con el DSN, usuario y contraseña preparados suele ser suficiente.

---

## Diferencias entre PDO y MySQLi (explicación breve y directa)

- MySQLi: interfaz específica para MySQL/MariaDB. Permite uso procedural y orientado a objetos. En algunos despliegues simples funciona sin ajustes adicionales.
- PDO: interfaz de acceso a múltiples bases de datos (MySQL/MariaDB, PostgreSQL, SQLite, etc.). Soporta named parameters y facilita portabilidad entre motores.