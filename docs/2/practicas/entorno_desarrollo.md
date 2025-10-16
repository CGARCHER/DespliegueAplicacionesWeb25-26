# 🐳 Práctica: Dockerización Full-Stack (Spring Boot + PHP + PostgreSQL)

## Objetivo

Configurar un entorno de desarrollo y producción utilizando Docker, integrando:

- Backend: Spring Boot (API REST)
- Frontend: PHP
- Base de datos: PostgreSQL
- Interfaz visual: pgAdmin

El propósito es entender cómo se conecta cada componente y diferenciar entre el entorno de desarrollo y el de producción.

---

## Desarrollo

### Entorno de desarrollo

- El backend (Spring Boot) se ejecuta desde el IDE local.
- Docker se utiliza para el frontend (PHP), la base de datos (PostgreSQL) y pgAdmin.
- El frontend PHP debe ser capaz de consumir la API REST de Spring Boot que está fuera de Docker, usando `host.docker.internal` o la alternativa de tu sistema operativo.
- El backend se conecta a la base de datos mediante la red interna de Docker (`db:5432`).
- Al modificar código Java, solo es necesario reiniciar el backend.
- Es fundamental distinguir cómo se comunican los servicios y cómo cambia esto respecto al entorno de producción.

---

### Entorno de producción

- Todos los servicios (Spring Boot, PHP, PostgreSQL y pgAdmin) se ejecutan en contenedores Docker.
- El backend se arranca dentro de su propio contenedor Docker.
    - Opción A: Usar multistage build para construir el `.jar` dentro del contenedor.
    - Opción B: Construir el `.jar` manualmente y copiarlo al contenedor.
- Los servicios se comunican entre sí usando los nombres de los servicios definidos en Docker Compose.
- Debes conocer la función y ventajas del multistage build y la diferencia entre preparar el `.jar` fuera o dentro del contenedor.

---

## Consideraciones finales

- Los servicios deben poder arrancarse y pararse de forma ordenada.
- Es importante saber cómo acceder a los logs de cada contenedor y cómo reconstruir solo el servicio necesario.
- Utiliza volúmenes persistentes para que los datos de la base de datos no se pierdan al reiniciar los contenedores.

---

## Resumen

| Entorno     | Backend           | Frontend | Base de datos | pgAdmin | Flujo de trabajo                                    |
|-------------|-------------------|----------|---------------|---------|-----------------------------------------------------|
| Desarrollo  | IDE local         | Docker   | Docker        | Docker  | Reiniciar backend para cambios; frontend y DB en Docker |
| Producción  | Docker (jar/multistage) | Docker   | Docker        | Docker  | Todo en Docker; rebuild necesario al cambiar backend |

---

## Guía rápida

- **Desarrollo:** Backend local, frontend y base de datos en Docker. Comunicación entre host y contenedores.
- **Producción:** Todo dockerizado. Backend se construye y ejecuta dentro de su contenedor. Comunicación interna por nombres de servicio.
- **Volúmenes:** Usa volúmenes persistentes para evitar pérdida de datos en la base de datos.
- **Logs y reconstrucción:** Consulta los logs de cada contenedor y reconstruye solo el servicio afectado cuando sea necesario.
  

*Con amor, _cgarcher_* ❤️