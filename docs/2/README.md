# Introducción a la virtualización

## Índice

1. [Introducción a la virtualización](#1-introducción-a-la-virtualización)
2. [Introducción y Conceptos Fundamentales](#2-introducción-y-conceptos-fundamentales)
3. [Contenedores vs. Máquinas Virtuales](#3-contenedores-vs-máquinas-virtuales)
4. [Arquitectura y Componentes Docker](#4-arquitectura-y-componentes-docker)
5. [Instalación en Windows](#5-instalación-en-windows)
6. [Comandos Esenciales y Gestión de Contenedores](#6-comandos-esenciales-y-gestión-de-contenedores)
7. [Imágenes y Dockerfile](#7-imágenes-y-dockerfile)
8. [Persistencia y Redes](#8-persistencia-y-redes)
9. [Compartición de Imágenes y Docker Hub](#9-compartición-de-imágenes-y-docker-hub)
10. [Orquestación con Docker Compose](#10-orquestación-con-docker-compose)
11. [Seguridad y Buenas Prácticas](#11-seguridad-y-buenas-prácticas)
12. [Alternativa: Podman y Licencias Empresariales](#12-alternativa-podman-y-licencias-empresariales)
13. [Mini Ejercicios Prácticos](#13-mini-ejercicios-prácticos)
14. [Referencias y Recursos](#14-referencias-y-recursos)
15. [**Curso recomendado**](#15-curso-recomendado)

---

## 1. Introducción a la virtualización

La **virtualización** es una tecnología que permite crear entornos simulados o virtuales sobre un sistema físico. Tradicionalmente, se ha usado para crear máquinas virtuales (VM) que ejecutan sistemas operativos completos sobre un hipervisor, aprovechando al máximo los recursos de hardware y facilitando la gestión y el aislamiento de aplicaciones.

Con el avance de la tecnología y las necesidades de eficiencia, han surgido alternativas como los **contenedores**, que permiten virtualizar a nivel de sistema operativo, compartiendo el kernel y ofreciendo un entorno aislado y ligero para ejecutar aplicaciones. Los contenedores, popularizados por herramientas como Docker, han revolucionado el desarrollo y despliegue de aplicaciones al facilitar la portabilidad, escalabilidad y automatización.

Esta documentación se centra en Docker y la virtualización de aplicaciones web mediante contenedores, pero es importante entender que la virtualización en general abarca desde máquinas virtuales tradicionales hasta tecnologías modernas de contenedores.

---

## 2. Introducción y Conceptos Fundamentales

Docker es una plataforma de código abierto que permite empaquetar, distribuir y ejecutar aplicaciones en contenedores. Los contenedores son ligeros y eficientes, ofreciendo portabilidad, aislamiento y flexibilidad para desarrollo y producción.

### ¿Qué es un contenedor?

Un **contenedor** es una unidad estándar de software que encapsula código y todas sus dependencias, permitiendo que la aplicación se ejecute de manera rápida y confiable en cualquier entorno. Los contenedores logran el aislamiento usando tecnologías de Linux como namespaces y cgroups, compartiendo el kernel del sistema operativo, pero manteniendo separados los procesos y recursos.

### Origen de los contenedores

La idea de contenedores proviene de mecanismos de aislamiento en sistemas Unix/Linux como `chroot` (1979), y evolucionó con tecnologías como LXC (Linux Containers, 2008). Docker popularizó el concepto en 2013, facilitando la gestión, distribución y despliegue a gran escala.

### ¿Qué es OCI?

**OCI (Open Container Initiative)** es un proyecto de estándar abierto fundado en 2015 para definir especificaciones sobre la creación, ejecución y distribución de contenedores. Su objetivo es garantizar la compatibilidad entre diferentes herramientas, como Docker, Podman, y runtimes como containerd y runc.

- [Especificación de imagen OCI](https://github.com/opencontainers/image-spec)
- [Especificación de runtime OCI](https://github.com/opencontainers/runtime-spec)

### Otros conceptos clave

- **Imagen**: Archivo inmutable compuesto por capas, que contiene el sistema de archivos y dependencias para crear contenedores.
- **Registro**: Repositorio centralizado donde se almacenan y comparten imágenes (ej: Docker Hub).
- **Orquestación**: Conjunto de herramientas para automatizar la gestión de múltiples contenedores (ej: Docker Compose, Kubernetes).
- **Microservicio**: Arquitectura que descompone aplicaciones en servicios pequeños e independientes, facilitando el uso de contenedores.

---

## 3. Contenedores vs. Máquinas Virtuales

| Característica      | Contenedores                  | Máquinas Virtuales               |
|---------------------|------------------------------|----------------------------------|
| Virtualización      | OS-level (núcleo compartido) | Hardware-level (núcleo propio)   |
| Peso y recursos     | Ligeros, MB                  | Pesadas (GB)                     |
| Arranque            | Segundos                     | Minutos                          |
| Portabilidad        | Alta                         | Limitada                         |
| Aislamiento         | Muy bueno, depende del kernel| Absoluto (SO y hardware)         |

- Contenedores: ideales para microservicios, CI/CD, desarrollo y pruebas.
- Máquinas Virtuales: útiles en sistemas legacy y máxima seguridad.

---

## 4. Arquitectura y Componentes Docker

- **Docker Engine**: Daemon central que gestiona imágenes, contenedores, redes y volúmenes.
- **CLI**: Interfaz de línea de comandos para operar Docker.
- **Imágenes**: Plantillas de solo lectura para crear contenedores.
- **Contenedores**: Instancias vivas y aisladas de una imagen.
- **Registro**: Repositorios como Docker Hub para compartir imágenes.
- **Volúmenes**: Para datos persistentes.
- **Redes**: Comunicación segura entre contenedores.

---

## 5. Instalación en Windows

1. Descargar Docker Desktop desde la [web oficial](https://www.docker.com/products/docker-desktop/).
2. Activar WSL2 o Hyper-V.
3. Instalar y verificar con:
   ```bash
   docker run hello-world
   ```
4. Añadir usuario al grupo `docker` (Linux).
5. Docker Desktop: desarrollo; Docker Engine (CLI): servidores Linux.

---

## 6. Comandos Esenciales y Gestión de Contenedores

- Crear y ejecutar:
  ```bash
  docker run <imagen>
  ```
- Listar contenedores:
  ```bash
  docker ps
  docker ps -a
  ```
- Detener/eliminar:
  ```bash
  docker stop <id>
  docker rm <id>
  ```
- Acceso interactivo:
  ```bash
  docker exec -it <id> /bin/bash
  ```
- Ver logs:
  ```bash
  docker logs <id>
  ```
- Otros: exponer puertos `-p`, variables `-e`, volúmenes `-v`.

---

## 7. Imágenes y Dockerfile

El `Dockerfile` define la construcción de imágenes personalizadas.

**Instrucciones clave**:  
`FROM`, `WORKDIR`, `COPY/ADD`, `RUN`, `ENV`, `EXPOSE`, `CMD`, `ENTRYPOINT`, `USER`, `VOLUME`

**Ejemplo:**
```Dockerfile
FROM python:3.12-alpine
WORKDIR /code
COPY . .
RUN pip install -r requirements.txt
EXPOSE 8080
CMD ["python", "app.py"]
```

**Buenas prácticas:**
- Minimizar capas.
- Usar imágenes ligeras (alpine).
- `.dockerignore` para ignorar archivos no necesarios.
- No almacenar credenciales en imágenes.

---

## 8. Persistencia y Redes

- **Volúmenes**:
  ```bash
  docker volume create datosweb
  ```
- **Bind mounts**: sincroniza carpetas del host y contenedor.
- **Redes**:
  ```bash
  docker network create midockerred
  docker run --network midockerred ...
  ```

---

## 9. Compartición de Imágenes y Docker Hub

- Buscar:
  ```bash
  docker search nginx
  ```
- Descargar:
  ```bash
  docker pull nginx:alpine
  ```
- Etiquetar y subir:
  ```bash
  docker tag app:latest miusuario/appweb:1.0
  docker push miusuario/appweb:1.0
  ```
- Registros privados disponibles.

---

## 10. Orquestación con Docker Compose

Define múltiples servicios en un archivo `docker-compose.yml` (YAML):

```yaml
version: '3.8'
services:
  db:
    image: mariadb:10.5
    environment:
      MYSQL_ROOT_PASSWORD: clave
      MYSQL_DATABASE: blog
    volumes:
      - datosdb:/var/lib/mysql
  web:
    image: wordpress:latest
    depends_on:
      - db
    ports:
      - "8080:80"
    environment:
      WORDPRESS_DB_HOST: db
volumes:
  datosdb:
```

**Comandos útiles:**
```bash
docker-compose up -d
docker-compose down -v
docker-compose logs
docker-compose exec web bash
```

---

## 11. Seguridad y Buenas Prácticas

- Usar usuario no root en Dockerfile.
- Imágenes siempre actualizadas.
- No incluir credenciales.
- Limitar puertos y capacidades.
- Verificar imágenes y analizar vulnerabilidades.

---

## 12. Alternativa: Podman y Licencias Empresariales

- **Podman**: compatible con Docker, sin daemon ni root.
- **Licencias Docker Desktop**: gratuito para docencia, usuarios y pequeñas empresas (<250 empleados o <10M $).
- **Docker Engine (CLI)**: libre y sin restricciones.

---

## 13. Mini Ejercicios Prácticos

### Ejercicio 1: Servidor Web PHP y Fichero Personalizado

1. Crear `info.php`:
    ```php
    <?php phpinfo(); ?>
    ```
2. Ejecutar:
    ```bash
    docker run -d -v $(pwd):/var/www/html -p 8080:80 php:8.2-apache
    ```
3. Abrir [http://localhost:8080/info.php](http://localhost:8080/info.php)

---

### Ejercicio 2: WordPress y MariaDB con Docker Compose

1. Crear `docker-compose.yml`:
    ```yaml
    version: '3.8'
    services:
      db:
        image: mariadb:10.5
        environment:
          MYSQL_ROOT_PASSWORD: clave
          MYSQL_DATABASE: wordpress
          MYSQL_USER: usuario
          MYSQL_PASSWORD: clave
        volumes:
          - datosdb:/var/lib/mysql
      web:
        image: wordpress:latest
        depends_on:
          - db
        ports:
          - "8080:80"
        environment:
          WORDPRESS_DB_HOST: db
          WORDPRESS_DB_USER: usuario
          WORDPRESS_DB_PASSWORD: clave
          WORDPRESS_DB_NAME: wordpress
    volumes:
      datosdb:
    ```
2. Ejecutar:
    ```bash
    docker-compose up -d
    ```
3. Abrir [http://localhost:8080](http://localhost:8080)

---

### Ejercicio 3: Imagen Node.js con Dockerfile

1. Crear `server.js`:
    ```js
    const http = require('http');
    http.createServer((req, res) => {
        res.end('¡Hola Docker!');
    }).listen(8080);
    ```
2. Crear `Dockerfile`:
    ```Dockerfile
    FROM node:20-alpine
    COPY server.js .
    EXPOSE 8080
    CMD ["node", "server.js"]
    ```
3. Construir y ejecutar:
    ```bash
    docker build -t servidor-node .
    docker run -d -p 8081:8080 servidor-node
    ```
4. Abrir [http://localhost:8081](http://localhost:8081)

---

## 14. Referencias y Recursos

- [Documentación Oficial Docker](https://docs.docker.com/)
- [Docker Hub](https://hub.docker.com/)
- [Guía rápida Docker Compose](https://docs.docker.com/compose/)
- [Podman](https://podman.io/)
- [Licencias Docker Desktop](https://www.docker.com/pricing/)
- [Open Container Initiative (OCI)](https://opencontainers.org/)
- [Historia de los contenedores](https://www.redhat.com/en/topics/containers/what-are-linux-containers)

---

## 15. Curso recomendado

Te recomiendo completar el curso de Docker de [Pablo Pereza](https://pabpereza.dev/docs/cursos/docker), que ofrece una visión práctica y muy completa sobre la tecnología de contenedores, imágenes, despliegue y buenas prácticas.  
**Es muy aconsejable verlo entero para asentar conceptos y ampliar tus conocimientos.**

---

### Autor

Repositorio creado por [CGARCHER](https://github.com/CGARCHER) para documentación y ejercicios de virtualización de aplicaciones web con Docker, orientado a estudiantes de 2DAW.

---