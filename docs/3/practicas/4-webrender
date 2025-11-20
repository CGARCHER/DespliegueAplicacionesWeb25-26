# Práctica: Dockerización y despliegue de una página web estática con Nginx y Render

Descripción
-----------
En esta práctica crearás una página web estática (HTML, CSS, JS), la servirás con Nginx dentro de un contenedor Docker, la subirás a GitHub y la desplegarás en Render con despliegue automático para comprobar un flujo básico de CI/CD.

Objetivos
---------
- Crear una página estática sencilla.
- Empaquetarla en una imagen Docker basada en `nginx:alpine`.
- Probar el contenedor en local.
- Subir el proyecto a GitHub.
- Desplegar en Render y verificar despliegues automáticos al hacer push.

Requisitos previos
------------------
- Docker instalado y en funcionamiento.
- Cuenta en GitHub.
- Cuenta en Render (https://render.com).
- Editor de texto (VS Code, etc.).
- Conocimientos básicos de HTML/CSS/JS.

Estructura del proyecto
-----------------------
Crea la carpeta del proyecto con esta estructura:

mi-web-docker/
├─ Dockerfile
├─ .dockerignore
└─ web/
   ├─ index.html
   ├─ styles.css
   └─ app.js

Archivos de ejemplo
-------------------

index.html (ejemplo mínimo)
```html
<!doctype html>
<html lang="es">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Mi web Dockerizada</title>
  <link rel="stylesheet" href="styles.css" />
</head>
<body>
  <main>
    <h1>Mi web Dockerizada con Nginx</h1>
    <p>Si ves esta página, Nginx en Docker está sirviendo los archivos estáticos correctamente.</p>
    <button id="btn">Haz clic</button>
    <p id="msg"></p>
  </main>
  <script src="app.js"></script>
</body>
</html>
```

styles.css (ejemplo)
```css
body {
  font-family: Arial, sans-serif;
  background: linear-gradient(135deg,#f0f4ff,#f7fff0);
  color: #222;
  display: flex;
  height: 100vh;
  align-items: center;
  justify-content: center;
  margin: 0;
}
main {
  text-align: center;
  padding: 2rem;
  border-radius: 12px;
  background: rgba(255,255,255,0.85);
  box-shadow: 0 6px 20px rgba(0,0,0,0.08);
  max-width: 600px;
}
button { padding: 0.5rem 1rem; cursor: pointer; }
```

app.js (ejemplo)
```javascript
document.getElementById('btn').addEventListener('click', () => {
  document.getElementById('msg').textContent = '¡Funciona! Desplegado con Docker y Render 🚀';
});
```

Dockerfile
----------
Crea un Dockerfile en la raíz del proyecto con este contenido:

```dockerfile
# Dockerfile
FROM nginx:alpine
# Copia los archivos estáticos a la ruta que Nginx sirve por defecto
COPY web/ /usr/share/nginx/html/
# Exponer el puerto 80
EXPOSE 80
# Arrancar nginx en primer plano
CMD ["nginx", "-g", "daemon off;"]
```

.dockerignore (recomendado)
---------------------------
```text
.git
node_modules
.DS_Store
```

Construir y ejecutar localmente
-------------------------------
1. Desde la raíz del proyecto (donde está el Dockerfile):
```bash
docker build -t mi-web-docker .
```

2. Ejecutar el contenedor y mapear el puerto 80 del contenedor al 8080 del host:
```bash
docker run -d --name mi-web -p 8080:80 mi-web-docker
```

3. Abrir en el navegador:
```
http://localhost:8080
```

Comandos útiles:
```bash
# Ver logs del contenedor
docker logs -f mi-web

# Parar y eliminar
docker stop mi-web
docker rm mi-web

# Eliminar imagen si quieres reconstruir desde cero
docker rmi mi-web-docker
```

Subir el proyecto a GitHub
--------------------------
1. Inicializa el repositorio y haz el primer commit:
```bash
git init
git add .
git commit -m "Inicial: web estática + Dockerfile"
```

2. Crea un repositorio en GitHub (por ejemplo `mi-web-docker`) desde la interfaz web.

3. Añade el remoto y sube la rama `main` (si tu rama local es `master`, cámbiala a `main` o empuja `master` según prefieras):
```bash
git branch -M main
git remote add origin https://github.com/<tu-usuario>/<tu-repo>.git
git push -u origin main
```

Desplegar en Render
-------------------
1. Inicia sesión en Render y conecta tu cuenta de GitHub (si no está conectada).
2. Crea un nuevo "Web Service".
3. Selecciona el repositorio que acabas de subir.
4. Elige la rama `main`.
5. Plan: Free (gratuito).
6. Render detectará automáticamente el Dockerfile.
7. Crea el servicio. Render iniciará una primera build y te dará una URL pública cuando termine.

Verás una URL de acceso a la app desplegada.

Verificar despliegues automáticos
--------------------------------
- Realiza un cambio sencillo (por ejemplo en index.html), commit y push:
```bash
git add web/index.html
git commit -m "Actualizar texto de bienvenida"
git push
```
- En Render, verás que se dispara un nuevo despliegue automáticamente. Al terminar, recarga la URL pública para ver la actualización.