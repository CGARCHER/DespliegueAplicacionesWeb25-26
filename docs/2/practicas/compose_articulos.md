# 2 . Guía Paso a Paso: Dockeriza tu Aplicación PHP MVC de Artículos

Este ejercicio te guiará para dockerizar una aplicación PHP bajo arquitectura MVC que gestiona artículos, utilizando MySQL como base de datos y PhpMyAdmin como herramienta de administración. El objetivo es crear un entorno profesional, portátil y fácilmente replicable usando Docker y Docker Compose.

---

## Objetivo

Que comprendas y experimentes el proceso de:
- Preparar tu código PHP para ejecutarse en contenedores.
- Crear y entender archivos de configuración Docker (`Dockerfile` y `docker-compose.yml`).
- Desplegar la aplicación y sus servicios relacionados de forma sencilla.
- Aplicar buenas prácticas en la organización y despliegue de proyectos PHP modernos.

---

## 1. Analiza la estructura de tu aplicación

Tu aplicación sigue el patrón MVC (Modelo-Vista-Controlador). La estructura típica es:

- **assets/**: Recursos estáticos (CSS, JS, imágenes).
- **config/**: Configuración de la aplicación (por ejemplo, acceso a BD).
- **controller/**: Lógica de controladores.
- **model/**: Lógica de los modelos de datos.
- **repository/**: Acceso y gestión de datos.
- **sql/**: Scripts de creación/llenado de base de datos.
- **view/**: Vistas y plantillas HTML.
- **.htaccess**: Configuración de Apache (URLs amigables).
- **Dockerfile**: Definición de la imagen personalizada (lo crearás tú).
- **docker-compose.yml**: Orquestación de los servicios (lo crearás tú).
- **index.php**: Punto de entrada principal.


---

## 2. ¿Qué es Docker y por qué usarlo?

**Docker** es una plataforma que permite empaquetar aplicaciones y todas sus dependencias en "contenedores". Esto facilita la portabilidad, escalabilidad y un despliegue consistente en cualquier entorno.

**Ventajas:**
- Evita el típico "en mi máquina funciona".
- Arranque/despliegue rápido de entornos completos.
- Facilita la colaboración entre desarrolladores.
- Replica entornos de producción fácilmente en local.

---

## 3. Crea el archivo `Dockerfile`

Este archivo indica cómo construir la imagen del contenedor para tu aplicación PHP.

```dockerfile
FROM php:8.2-apache

# Instala extensiones necesarias de PHP para MySQL
RUN docker-php-ext-install mysqli pdo pdo_mysql

# Copia el código fuente al contenedor
COPY . /var/www/html/

# Habilita mod_rewrite para URLs amigables
RUN a2enmod rewrite

EXPOSE 80
```

- **FROM**: Indica la base de la imagen (PHP 8.2 + Apache).
- **RUN docker-php-ext-install**: Instala extensiones PHP para MySQL.
- **COPY**: Copia tu proyecto al servidor web del contenedor.
- **a2enmod rewrite**: Habilita redirección de URLs (importante para el MVC).
- **EXPOSE**: Expone el puerto por defecto del servidor web.

Guarda este archivo como `Dockerfile` en la raíz del proyecto.

---

## 4. Crea el archivo `docker-compose.yml`

Este archivo define y configura los servicios necesarios: web, base de datos y PhpMyAdmin.

```yaml
version: "3.9"

services:
  web:
    build: .
    ports:
      - "8084:80"
    volumes:
      - .:/var/www/html
    depends_on:
      - db

  db:
    image: mysql:8.0
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: root123
      MYSQL_DATABASE: productos
      MYSQL_USER: usuario_app
      MYSQL_PASSWORD: clave_app
    ports:
      - "3306:3306"
    volumes:
      - ./sql:/docker-entrypoint-initdb.d

  phpmyadmin:
    image: phpmyadmin/phpmyadmin
    restart: always
    ports:
      - "8085:80"
    environment:
      PMA_HOST: db
      PMA_PORT: 3306
    depends_on:
      - db
```

**Explicación:**
- **web**: Tu aplicación PHP, expuesta en el puerto 8084.
- **db**: Contenedor de MySQL 8, base de datos `productos` y usuario `usuario_app`.
- **phpmyadmin**: Herramienta web para gestionar MySQL, accesible en el puerto 8085.
- **volumes**: Permite que los cambios en tu código fuente se reflejen inmediatamente en el contenedor (`.:/var/www/html`).
- **depends_on**: Garantiza que la base de datos se inicie antes que la aplicación.

Guarda este archivo como `docker-compose.yml` en la raíz del proyecto.

---

## 5. Scripts de inicialización de base de datos

Coloca un script SQL en la carpeta `sql/` para que la base de datos se cree automáticamente con tablas y datos básicos al arrancar el contenedor.

---

## 6. Arranca los contenedores

Para levantar todos los servicios definidos en `docker-compose.yml`, abre una terminal en la raíz de tu proyecto y ejecuta:

```bash
docker-compose up --build
```

- El parámetro `--build` fuerza la reconstrucción de la imagen por si has hecho cambios en el `Dockerfile` o en las dependencias.
- Docker Compose creará los contenedores necesarios (PHP, MySQL y PhpMyAdmin) y enlazará sus redes y volúmenes.

---

### ¿Qué hacer si cambias la estructura de la base de datos (SQL)?

Si necesitas modificar la estructura de la base de datos (por ejemplo, añadir tablas o columnas nuevas), simplemente edita o añade los scripts SQL en la carpeta `sql/` de tu proyecto.

**Pasos recomendados:**
1. Modifica los archivos `.sql` dentro de la carpeta `sql/`.
2. Elimina los volúmenes de datos de MySQL para que al reiniciar el contenedor, MySQL vuelva a importar desde el SQL actualizado:
    ```bash
    docker-compose down -v
    ```
   El parámetro `-v` elimina los volúmenes asociados (¡esto borra todos los datos!).
3. Vuelve a levantar los contenedores:
    ```bash
    docker-compose up --build
    ```

> **¡Atención!** Este proceso borra por completo la base de datos, así que sólo hazlo en desarrollo, nunca en producción.

---

### ¿Puedo modificar el código PHP mientras Docker está corriendo?

¡Sí! Gracias a la configuración de volúmenes en `docker-compose.yml`:

```yaml
    volumes:
      - .:/var/www/html
```

Los archivos de tu proyecto están montados directamente dentro del contenedor. Cada vez que cambies cualquier archivo PHP, HTML, CSS, etc., los cambios se reflejan automáticamente en la aplicación, sin necesidad de reiniciar el contenedor.

Esto te permite desarrollar y probar en tiempo real, igual que si trabajaras en tu propio equipo, pero con todas las ventajas de un entorno controlado y replicable.

---

## 7. Comprueba el funcionamiento

- **Aplicación web:** [http://localhost:8084](http://localhost:8084)
- **PhpMyAdmin:** [http://localhost:8085](http://localhost:8085)
    - Usuario: `usuario_app`
    - Contraseña: `clave_app`
    - Servidor: `db` (o `localhost`)

---

## 8. Configura tu aplicación PHP para usar un usuario NO root

**Por seguridad y buenas prácticas**, nunca debes usar el usuario `root` de la base de datos en tus aplicaciones. El usuario root tiene todos los privilegios y si tu aplicación fuera vulnerada, el atacante podría borrar, modificar o robar toda la información de todas las bases de datos.

### - Crea/edita el archivo `config/config.ini` así:


### ¿Por qué es importante?

- Si usas root, cualquier vulnerabilidad en la aplicación puede comprometer toda la base de datos.
- Usando un usuario con permisos limitados, sólo tendrá acceso a la base de datos y operaciones necesarias para funcionar.
- Es una práctica profesional y estándar en seguridad.

---

## 9. Detener los servicios

Cuando termines de trabajar, puedes parar y eliminar los contenedores ejecutando:

```bash
docker-compose down
```

---

## 10. Buenas prácticas y consejos

- Usa variables de entorno para contraseñas y configuraciones sensibles.
- Controla los logs de tus contenedores con `docker-compose logs` o `docker logs`.
- Consulta el estado de los contenedores con `docker ps`.
- Puedes reiniciar servicios individuales con `docker-compose restart web` (o `db`, `phpmyadmin`).

---

## 11. Consulta la documentación oficial de MySQL Docker

Para más detalles sobre cómo configurar el contenedor de MySQL (variables de entorno, usuarios, persistencia, backups, etc.), revisa la documentación oficial en Docker Hub:

- [https://hub.docker.com/_/mysql](https://hub.docker.com/_/mysql)

---

## 12. ¿Qué habrás aprendido?

- Cómo preparar y adaptar una aplicación PHP para ejecutarse en Docker.
- A crear y entender archivos de configuración Docker.
- A desplegar entornos de desarrollo modernos de forma sencilla y replicable.
- A utilizar herramientas profesionales (PhpMyAdmin, volúmenes, dependencias entre servicios).
- A implementar buenas prácticas de seguridad en el acceso a la base de datos.

---

## Requisitos previos

- Tener instalado Docker  ([descarga aquí](https://docs.docker.com/get-docker/)).

---

¡Sigue estos pasos, experimenta, modifica y aprende! Docker es una herramienta fundamental en el desarrollo moderno, y dominarla te abrirá muchas puertas en el mundo profesional .

*Con amor, _cgarcher_* ❤️
