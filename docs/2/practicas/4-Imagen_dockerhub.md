# 4. Práctica: Publicar una aplicación Astro en Docker Hub

Este proyecto muestra cómo crear una aplicación web sencilla usando **Astro**, contenerizarla con **Docker** y publicarla en **Docker Hub**.

---

## 1. Crear el Proyecto Astro

```bash
mkdir astro-nombres
cd astro-nombres
npm create astro@latest
```

Responde a las preguntas:
- Nombre del proyecto: astro-nombres
- Plantilla: minimal
- TypeScript: Yes (opcional)
- Instalar dependencias: Yes

Para probar la aplicación en local:

```bash
npm run dev
```

Abre tu navegador en [http://localhost:4321](http://localhost:4321).

---

## 2. Editar el Listado de Nombres

Edita el archivo `src/pages/index.astro` para que aparezcan los **nombres de todos los alumnos de clase**. Por ejemplo:

```astro
---
const nombres = ["Juan", "Lucía", "Pedro", "Carmen", "Sofía"];
// Sustituye estos nombres por los de tus compañeros de clase
---

<html lang="es">
  <head>
    <meta charset="UTF-8" />
    <title>Listado de Nombres</title>
  </head>
  <body>
    <h1>Listado de Nombres</h1>
    <ul>
      {nombres.map((nombre) => <li>{nombre}</li>)}
    </ul>
  </body>
</html>
```

Antes de continuar, asegúrate de que el listado contiene los nombres reales de todos los alumnos de la clase.

---

## 3. Crear el Dockerfile

En la raíz del proyecto, crea un archivo llamado `Dockerfile` con este contenido:

```dockerfile
FROM node:20 AS build
WORKDIR /app

COPY package*.json ./
RUN npm install

COPY . .
RUN npm run build

FROM node:20-alpine
WORKDIR /app

RUN npm install -g serve
COPY --from=build /app/dist ./dist

EXPOSE 5000
CMD ["serve", "-s", "dist", "-l", "5000"]
```

---

## 4. Construir la Imagen Docker

Asegúrate de estar en la carpeta raíz del proyecto. Construye la imagen usando tu nombre de usuario de Docker Hub:

```bash
docker build -t tuusuario/astro-nombres:1.0 .
```

Reemplaza `tuusuario` por tu usuario real de Docker Hub.

---

## 5. Probar la Imagen Localmente

Ejecuta el contenedor en tu máquina local:

```bash
docker run -d -p 8080:5000 tuusuario/astro-nombres:1.0
```

Abre [http://localhost:8080](http://localhost:8080) en tu navegador y deberías ver el listado de los nombres de los alumnos de la clase.

Para detener el contenedor:

```bash
docker ps             # Busca el CONTAINER ID
docker stop <ID>      # Detén el contenedor usando su ID
```

---

## 6. Subir la Imagen a Docker Hub

### 6.1 Iniciar Sesión en Docker Hub

```bash
docker login
```
Sigue las instrucciones e ingresa tus credenciales.

### 6.2 (Opcional) Etiquetar la Imagen

Si olvidaste poner el tag correcto al construir, puedes etiquetarla así:

```bash
docker tag <IMAGE_ID> tuusuario/astro-nombres:1.0
```
Puedes ver el `<IMAGE_ID>` con `docker images`.

### 6.3 Subir la Imagen

```bash
docker push tuusuario/astro-nombres:1.0
```
Esto subirá la imagen a tu cuenta de Docker Hub.

---

## 7. Intercambio y Pruebas entre Compañeros

Cuando hayas subido tu imagen a Docker Hub, indícale a tu compañero/a el nombre de tu imagen (por ejemplo: `tuusuario/astro-nombres:1.0`).  
Pídele a tu compañero que descargue y pruebe tu imagen con el siguiente comando:

```bash
docker run -d -p 8080:5000 tuusuario/astro-nombres:1.0
```

Tu compañero debe abrir [http://localhost:8080](http://localhost:8080) y comprobar que ve el listado de los alumnos.

---

## 8. Resumen de Comandos

```bash
# Crear y probar proyecto Astro
npm create astro@latest
npm run dev

# Editar el listado de nombres (con los alumnos de clase)

# Construir imagen Docker
docker build -t tuusuario/astro-nombres:1.0 .

# Probar en local
docker run -d -p 8080:5000 tuusuario/astro-nombres:1.0

# Login Docker Hub
docker login

# (Opcional) Etiquetar imagen
docker tag <IMAGE_ID> tuusuario/astro-nombres:1.0

# Subir imagen a Docker Hub
docker push tuusuario/astro-nombres:1.0

# Probar la imagen de un compañero
docker run -d -p 8080:5000 nombrecompañero/astro-nombres:1.0
```

---

## 9. Enlaces útiles

- [Astro Docs](https://docs.astro.build/)
- [Docker Docs](https://docs.docker.com/)
- [Docker Hub](https://hub.docker.com/)
