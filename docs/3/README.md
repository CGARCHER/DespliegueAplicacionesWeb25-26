# Despliegue de Aplicaciones Web — UT3 Arquitectura Web y Fundamentos

Descripción
-----------
El Despliegue de Aplicaciones Web es el proceso que lleva una aplicación desde el código fuente a un entorno accesible por usuarios finales. No se trata solo de "subir archivos": implica garantizar estabilidad, seguridad, rendimiento y escalabilidad. En este tema veremos la estructura detrás de cada acción web: modelo cliente‑servidor, arquitecturas del backend, protocolo HTTP/HTTPS, APIs, servidores, contenedores, orquestación y prácticas modernas de CI/CD.

1) Componentes clave y roles en el desarrollo web
-------------------------------------------------
Front‑end (lado del cliente)
- Tecnologías: HTML, CSS, JavaScript; frameworks comunes: React, Vue, Angular, Svelte.
- Responsabilidades: interfaz, interacción, validaciones cliente, consumo de APIs, rendimiento (lazy loading), accesibilidad y SEO.
- Consideración: renderizado en cliente vs servidor (SSR) según SEO y rendimiento.

Back‑end (lado del servidor)
- Tecnologías habituales: Node.js/Express, Django/Flask, Spring, ASP.NET, PHP/Laravel.
- Responsabilidades: lógica de negocio, persistencia, autenticación/autorización, validación y exposición de APIs (REST/GraphQL/gRPC).
- Buenas prácticas: separar responsabilidades y exponer funcionalidades vía APIs.

Comunicación cliente‑servidor
- Formatos: JSON, (XML en casos legacy).
- Retos: CORS, manejo de errores, timeouts, límites de tamaño.

2) Modelos de arquitectura de software
--------------------------------------
Monolítica
- Todos los componentes en un mismo despliegue.
- Pros: simple arranque y despliegue inicial.
- Contras: escalado y mantenimiento más difíciles.
- Ideal para: proyectos pequeños, MVPs, prácticas.

Arquitectura por capas
- Capas típicas: presentación → negocio → datos.
- Pros: modularidad, mantenibilidad.
- Uso: aplicaciones empresariales.

Microservicios
- Servicios pequeños y autónomos que se comunican por APIs o mensajería.
- Pros: despliegues independientes, escalado por servicios.
- Contras: mayor complejidad operativa (observabilidad, coordinación).
- Uso: sistemas complejos (p.ej. Netflix).

Serverless (FaaS)
- El proveedor gestiona la infraestructura; ejecutas funciones.
- Pros: escalado automático y pago por uso.
- Uso: tareas event‑driven, cargas impredecibles.

Patrón MVC (Modelo‑Vista‑Controlador)
- Modelo: acceso y manipulación de datos.
- Vista: presentación (HTML, plantillas).
- Controlador: recibe peticiones, orquesta Modelo y Vista.
- Flujo: petición → router → controlador → modelo → vista/JSON → cliente.
- Nota: en este curso el primer proyecto seguirá server‑rendered (MVC clásico).

3) Protocolo HTTP/HTTPS y APIs
------------------------------
HTTP (base de la web)
- Verbos: GET, POST, PUT, PATCH, DELETE.
- Idempotencia: GET/PUT/DELETE idempotentes; POST no.
- Códigos de estado comunes:
    - 2xx: éxito (200 OK, 201 Created, 204 No Content)
    - 4xx: error cliente (400, 401, 403, 404, 422)
    - 5xx: error servidor (500, 502, 503)
- Encabezados útiles: Authorization, Content-Type, Accept, Cache-Control, ETag.

HTTPS
- Cifrado TLS/SSL; uso de certificados (Let’s Encrypt para prácticas).
- Siempre HTTPS en producción; en desarrollo se puede usar ngrok o servicios con TLS incorporado.

APIs y estilos de comunicación
- REST: recursos identificables por URL y operaciones CRUD vía HTTP; uso mayoritario de JSON.
- GraphQL: cliente pide exactamente los campos que necesita; reduce over/under‑fetching.
- gRPC: RPC de alto rendimiento, basado en HTTP/2 y Protobuf (ideal para comunicaciones internas).
- WebSocket: canal bidireccional persistente para tiempo real.
- Seguridad de APIs: autenticación (JWT, OAuth2), autorización, rate limiting, validación y saneamiento de entrada.

4) Servidores, despliegue, contenedores y CI/CD
----------------------------------------------
Servidores y reverse proxies
- Nginx y Apache: sirven estáticos, actúan como reverse proxy, terminan TLS y pueden hacer load balancing.
- Servidor de aplicaciones (p.ej. Tomcat): específico para Java y Java EE.

Caching y CDN
- Cache de navegador (headers), CDN para estáticos y cache inverso (Varnish) para reducir latencia y carga.

Escalabilidad
- Vertical (scale up): más CPU/RAM en la misma máquina.
- Horizontal (scale out): más instancias detrás de un balanceador.
- Balanceo: Nginx, HAProxy o balanceadores del cloud.

Contenedores y orquestación
- Docker: empaqueta la app + dependencias en imágenes reproducibles.
- Kubernetes: orquesta despliegues, réplicas, autoescalado y redes entre contenedores.
- Conceptos clave en Kubernetes: Pod, Deployment, Service, Ingress, ConfigMap, Secret.

CI/CD (Integración Continua / Despliegue Continuo)
- CI: build + tests automáticos en cada push (GitHub Actions, GitLab CI, Jenkins).
- CD: despliegue a staging/production; estrategias: rolling, blue‑green, canary.
- Recomendación: incluir pruebas automáticas y posibilidad de rollback.

Observabilidad y operación
- Logs centralizados (ELK/EFK), métricas (Prometheus), dashboards (Grafana).
- Health checks y probes en Kubernetes.
- Backups y migraciones de bases de datos: planificarlas y probarlas.

### Autor
En construcción, falta detalla mucho y añadir ejercicios para el tema.
Repositorio creado por [CGARCHER](https://github.com/CGARCHER).
---


