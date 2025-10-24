# 5. Múltiples configuraciones docker

Este proyecto es una plantilla **MVC-PHP** preparada para trabajar con **Docker Compose** en dos entornos diferenciados: **local** y **desarrollo** (dev), cada uno con su propia base de datos y datos de ejemplo.
El objetivo de la práctica es **gestionar contenedores en diferentes entornos** de forma sencilla y didáctica.

---

## 1. Estructura de archivos relevante

```
/
├── Dockerfile
├── Makefile
├── docker-compose.yml
├── docker-compose.local.yml
├── docker-compose.dev.yml
├── sql/
│   ├── local/
│   │   └── products.sql
│   └── dev/
│       └── products.sql
├── ...
```

### ¿Para qué sirve cada archivo?

- **Dockerfile**: Imagen personalizada de la aplicación PHP.
- **Makefile**: Facilita la ejecución de comandos para gestionar los entornos y contenedores con instrucciones sencillas.
- **docker-compose.yml**: Configuración base común para todos los entornos. Define servicios principales como la aplicación y la base de datos.
- **docker-compose.local.yml**: Sobreescribe la configuración base para el entorno local (tu PC), usando MariaDB y montando el código fuente.
- **docker-compose.dev.yml**: Sobreescribe la configuración base para el entorno de desarrollo compartido (por ejemplo, integración continua), usando MySQL 8 y cargando otros datos SQL.
- **sql/local/products.sql**: Script de inicialización de la base de datos para el entorno local.
- **sql/dev/products.sql**: Script de inicialización de la base de datos para el entorno de desarrollo.

---

## 2. Configuración base: `docker-compose.yml`

Define los servicios generales que comparten ambos entornos (la aplicación y la base de datos), así como el volumen persistente para la base de datos.

```yaml
version: '3.8'
services:
  app:
    build: .
    ports:
      - "8084:80"
    depends_on:
      - db
  db:
    environment:
      MYSQL_ROOT_PASSWORD: password
      MYSQL_DATABASE: products_db
      MYSQL_USER: usuario_app
      MYSQL_PASSWORD: clave_app
    volumes:
      - db_data:/var/lib/mysql
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

volumes:
  db_data:
```

**¿Qué hace este archivo?**
- Declara los servicios que se van a levantar (app y db).
- Define variables de entorno comunes.
- Define el volumen `db_data` para guardar los datos de la base y que no se pierdan al reiniciar el contenedor.

---

## 3. Configuración local: `docker-compose.local.yml`

Añade o modifica opciones para el entorno local de desarrollo.

```yaml
version: '3.8'
services:
  db:
    image: mariadb:10.5
    ports:
      - "3306:3306"
    volumes:
      - db_data:/var/lib/mysql
      - ./sql/local/products.sql:/docker-entrypoint-initdb.d/products.sql
  app:
    volumes:
      - ./:/var/www/html
```

**¿Qué sobreescribe o agrega este archivo?**
- Cambia el motor de base de datos a **MariaDB**.
- Expone el puerto 3306 para poder acceder a la base desde tu PC.
- Monta el SQL de la carpeta `local` para inicializar la base con datos de prueba para desarrollo.
- Monta el código fuente localmente, útil para desarrollo ágil (los cambios se ven al instante).

---

## 4. Configuración para desarrollo compartido: `docker-compose.dev.yml`

Configura el entorno de desarrollo compartido, integración, CI/CD, etc.

```yaml
version: '3.8'
services:
  db:
    image: mysql:8.0
    volumes:
      - db_data:/var/lib/mysql
      - ./sql/dev/products.sql:/docker-entrypoint-initdb.d/products.sql
    # No se expone el puerto 3306
  app:
  # No se monta código fuente local
```

**¿Qué cambia respecto al entorno local?**
- Usa **MySQL 8.0**, más parecido a producción.
- No expone el puerto de la base de datos (más seguro).
- Usa un SQL de inicialización diferente (`dev`) para simular datos en ese entorno.
- No monta el código fuente local, ideal para pruebas de integración y evitar cambios accidentales.

---

## 5. Scripts SQL para cada entorno

### sql/local/products.sql

Contiene la creación de la tabla y datos de ejemplo para pruebas en local.

```sql
CREATE TABLE `PRODUCTS` (
  `cod` INT AUTO_INCREMENT PRIMARY KEY,
  `short_name` varchar(20) NOT NULL,
  `pvp` decimal(5,2) NOT NULL,
  `nombre` varchar(100) NOT NULL
) ENGINE=InnoDB DEFAULT CHARSET=latin1;

INSERT INTO `PRODUCTS` (`cod`, `short_name`, `pvp`, `nombre`) VALUES
(1, 'HD', '100.00', 'Samsung'),
(2, 'NOTEBOOK', '800.00', 'Lenovo IdeaPad'),
(3, 'MOUSE', '20.00', 'Logitech M90');
```

### sql/dev/products.sql

Datos y estructura para el entorno de desarrollo.

```sql
CREATE TABLE `PRODUCTS` (
  `cod` INT AUTO_INCREMENT PRIMARY KEY,
  `short_name` varchar(20) NOT NULL,
  `pvp` decimal(5,2) NOT NULL,
  `nombre` varchar(100) NOT NULL
) ENGINE=InnoDB DEFAULT CHARSET=latin1;

INSERT INTO `PRODUCTS` (`cod`, `short_name`, `pvp`, `nombre`) VALUES
(1, 'SSD', '400.00', 'BENQ'),
(2, 'PIXEL 10', '999.99', 'dispositivo google'),
(3, 'iPad Pro', '900.00', 'Apple iPad Pro 9');
```

---

## 6. Makefile: gestión fácil de los entornos

El **Makefile** permite ejecutar comandos de Docker Compose de forma sencilla y sin errores de tipeo.
¡Muy útil para quienes se inician en Docker!

Guarda este archivo como `Makefile` en la raíz del proyecto.

```makefile
# --------------------------------------------------------
# Makefile básico - MVC-PHP-Producto-Docker
# Permite levantar los entornos LOCAL y DEV fácilmente
# --------------------------------------------------------

# Archivos base
BASE = docker-compose.yml
LOCAL = docker-compose.local.yml
DEV = docker-compose.dev.yml

# Levantar entorno LOCAL
up-local:
	docker compose -f $(BASE) -f $(LOCAL) up -d --build

# Levantar entorno DEV
up-dev:
	docker compose -f $(BASE) -f $(DEV) up -d --build

# Eliminar entorno local
down-local:
	docker compose -f $(BASE) -f $(LOCAL) down -v

# Eliminar entorno dev
down-dev:
	docker compose -f $(BASE) -f $(DEV) down -v
```

---

### ¿Qué hace cada comando del Makefile?

- **make up-local**
  Levanta el entorno **local**: MariaDB, datos locales, puerto 3306 expuesto y código montado.

- **make up-dev**
  Levanta el entorno **dev**: MySQL 8, datos de desarrollo, sin exponer puertos y sin montar código.

- **make down-local**
  Detiene y elimina los contenedores del entorno de local.
- 
- **make down-dev**
  Detiene y elimina los contenedores del entorno de dev.

---

## 7. ¿Cómo probar y gestionar los entornos?

0. **Instalación de `make` en Windows usando winget** (En caso de no tenerlo ya de antes)
Para instalar el programa **make** en Windows utilizando el administrador de paquetes de Windows (**winget**), ejecuta el siguiente comando en la terminal:

```sh
winget install ezwinports.make
```
Esto descargará e instalará `make` en tu sistema Windows, facilitando la gestión y automatización de tareas de compilación


1. **Levanta el entorno local (desarrollo en tu PC):**
 
    

   ```bash
   make up-local
   ```

   - Acceso a la app en [http://localhost](http://localhost).
   - Puedes conectarte a MariaDB en `localhost:3306` (usuario root, password password).
   - Cambios en el código se reflejan al instante.

2. **Levanta el entorno de desarrollo (integración, compartido, CI):**

   ```bash
   make up-dev
   ```

   - No expone la base de datos.
   - Datos y configuración realistas para pruebas de integración.

3. **Apaga cualquier entorno:**

   ```bash
   make down
   ```

4. **Reconstruye el entorno local o dev:**

   ```bash
   make rebuild-local
   # o
   make rebuild-dev
   ```

---

## 8. Resumen rápido de entornos

| Entorno     | Comando Makefile             | Motor DB   | Datos de ejemplo         | Archivo SQL cargado           | Acceso puerto DB | Código fuente montado |
|-------------|------------------------------|------------|-------------------------|-------------------------------|------------------|----------------------|
| LOCAL       | `make up-local`              | MariaDB    | products.sql (local)    | sql/local/products.sql        | Sí (3306)        | Sí                   |
| DEV         | `make up-dev`                | MySQL 8.0  | products.sql (dev)      | sql/dev/products.sql          | No               | No                   |

---

## 9. Consejos y buenas prácticas

- Puedes crear más carpetas y SQL según necesites más tablas o datos.
- Si cambias el nombre de la base de datos o usuario, ajusta los archivos SQL y los docker-compose.
- Usa el Makefile para evitar errores y agilizar la gestión de contenedores.
- Revisa los logs de los contenedores si tienes problemas de inicialización (`docker compose logs`).

---

**¡Ya puedes gestionar y probar tu proyecto MVC-PHP en dos entornos diferenciados, de forma fácil y profesional!**