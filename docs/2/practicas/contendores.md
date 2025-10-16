# 1. Crear contenedores MySQL y phpMyAdmin con Docker

## ¿Qué estamos haciendo?
Vamos a crear dos contenedores Docker:
- **MySQL:** Base de datos relacional para almacenar la información.
- **phpMyAdmin:** Cliente web para administrar MySQL fácilmente desde el navegador.

Usamos puertos personalizados para evitar conflictos con otros servicios.

---

## 1. Descargar las imágenes oficiales de MySQL y phpMyAdmin

```bash
docker pull mysql:8.0
docker pull phpmyadmin
```
**Explicación:**  
Estos comandos descargan las imágenes oficiales de MySQL y phpMyAdmin desde Docker Hub.  
No es obligatorio ejecutarlos antes de crear el contenedor, ya que si usas directamente `docker run`, Docker descargará automáticamente la imagen si no la encuentra en tu sistema local.

---

## 2. Crear el contenedor de MySQL en el puerto 3307

```bash
docker run --name mysql-db -e MYSQL_ROOT_PASSWORD=root -e MYSQL_DATABASE=mi_db -p 3307:3306 -d mysql:8.0
```
**Explicación:**
- `--name mysql-db`: Asigna nombre al contenedor.
- `-e MYSQL_ROOT_PASSWORD=root`: Define la contraseña del usuario root de MySQL (¡cámbiala en producción!).
- `-e MYSQL_DATABASE=mi_db`: Crea una base de datos inicial llamada `mi_db`.
- `-p 3307:3306`: Expone el puerto 3306 del contenedor como 3307 en tu máquina.
- `-d`: Ejecuta en segundo plano.
- `mysql:8.0`: Especifica la imagen a usar.

---

## 3. Verificar que el contenedor está en ejecución

```bash
docker ps
```
**Explicación:**  
Muestra la lista de contenedores activos. Así verificas que MySQL está corriendo y en el puerto correcto.

### Información adicional

Si quieres ver **todos los contenedores** (en ejecución y detenidos), puedes usar:

```bash
docker ps -a
```
Esto mostrará tanto los contenedores activos como los que están detenidos, eliminados o que fallaron al iniciar. Es útil para revisar el estado general de tus contenedores, ver cuándo se crearon y otros detalles administrativos.

---

## 4. Crear el contenedor de phpMyAdmin en el puerto 8081

```bash
docker run --name phpmyadmin -d -p 8081:80 --link mysql-db -e PMA_HOST=mysql-db phpmyadmin
```
**Explicación:**
- `--name phpmyadmin`: Asigna nombre al contenedor.
- `-d`: Ejecuta en segundo plano.
- `-p 8081:80`: Expone el puerto 80 del contenedor (phpMyAdmin) como 8081 en tu máquina.
- `--link mysql-db`: Conecta phpMyAdmin con el contenedor MySQL.
- `-e PMA_HOST=mysql-db`: Indica el host de MySQL (nombre del contenedor creado antes).
- `phpmyadmin`: Usa la imagen oficial.

---

## 5. Acceder a phpMyAdmin desde el navegador

Abre tu navegador y entra en:
```
http://localhost:8081
```
**Explicación:**  
Aquí verás la interfaz web de phpMyAdmin.
- Usuario: `root`
- Contraseña: La que configuraste anteriormente (`root`).

---
*Con amor, _cgarcher_* ❤️
