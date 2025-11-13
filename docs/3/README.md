# Despliegue de Aplicaciones Web — UT3 Arquitectura Web y Fundamentos

# Tema 3 — Arquitectura Web y Fundamentos del Despliegue

Este tema pertenece al módulo profesional **Despliegue de Aplicaciones Web** del ciclo formativo de **2º de Desarrollo de Aplicaciones Web (DAW)**.  
Su finalidad es que el alumnado adquiera una visión completa de cómo se **diseña, configura, publica y mantiene una aplicación web** en un entorno de servidor real.


## 📚 Contenido

- [3.1 Introducción al Despliegue de Aplicaciones Web](#3-1-introducción-al-despliegue-de-aplicaciones-web)
- [3.2 Front-end vs Back-end y tipos de aplicaciones](#3-2-front-end-vs-back-end-y-tipos-de-aplicaciones)
- [3.3 Arquitecturas web y su impacto en el despliegue](#3-3-arquitecturas-web-y-su-impacto-en-el-despliegue)
- [3.4 Protocolos y comunicación (HTTP/HTTPS, APIs)](#3-4-protocolos-y-comunicación-httphttps-apis)
- [3.5 Tecnologías para aplicaciones dinámicas](#3-5-tecnologías-para-aplicaciones-dinámicas)
- [3.6 Plataformas y entornos de ejecución](#3-6-plataformas-y-entornos-de-ejecución)
- [3.7 Servidores web y de aplicaciones](#3-7-servidores-web-y-de-aplicaciones)
- [3.8 Estrategias y patrones de despliegue](#3-8-estrategias-y-patrones-de-despliegue)
- [3.9 Seguridad y monitorización](#3-9-seguridad-y-monitorización)


## 3.1 Introducción al Despliegue de Aplicaciones Web


El **desarrollo web moderno** es un ámbito en **constante evolución** que abarca la **creación y gestión** de sitios web y aplicaciones que funcionan a través de Internet. En la actualidad, el desarrollo no solo se enfoca en la funcionalidad, sino también en la manera en que estas aplicaciones se pondrán a disposición de los usuarios, un proceso crucial conocido como **despliegue**. Este proceso es esencial para la viabilidad de cualquier proyecto web, ya que permite que la aplicación pase del entorno de desarrollo a un entorno de producción, donde será accesible para los usuarios finales.

Los **principales objetivos** del despliegue son garantizar la accesibilidad, la estabilidad, la escalabilidad y la seguridad de las aplicaciones. Un despliegue eficiente contribuye a una mayor rapidez en el **Time-to-Market**, lo que permite a las empresas lanzar productos con mayor agilidad. Facilita la iteración constante y la entrega continua de nuevas funciones y mejoras, algo esencial para adaptarse a las demandas del mercado y de los usuarios. La **automatización** de los procesos de despliegue disminuye los errores humanos y optimiza la eficiencia, liberando a los equipos de desarrollo para tareas más estratégicas. Un despliegue eficaz potencia la competitividad de una empresa al ofrecer un servicio confiable y de alta calidad. Además, la **documentación completa** de todos los procesos de despliegue resulta indispensable para asegurar que puedan reproducirse, facilitar la resolución de problemas y apoyar la capacitación de nuevos miembros del equipo.

![img](images/anatomia_web.gif)


## 3.2 Front-end vs Back-end


En el desarrollo web moderno, el trabajo se divide principalmente entre dos áreas: **Front-end** y **Back-end**, que representan la parte visible y la parte interna de una aplicación, respectivamente.

El **Front-end** es todo lo que el usuario ve y con lo que interactúa directamente en su navegador. Se encarga del diseño, la estructura visual y la experiencia de usuario. Utiliza tecnologías como **HTML**, **CSS** y **JavaScript**, junto con frameworks y librerías como **React**, **Vue**, **Angular** o **Svelte**, para crear interfaces dinámicas y adaptables a distintos dispositivos. Además, el desarrollador Front-end debe preocuparse por la usabilidad, la accesibilidad y el posicionamiento en buscadores (SEO).

El **Back-end**, por su parte, es la parte que se ejecuta en el servidor y se ocupa de la **lógica interna** de la aplicación: procesar peticiones, conectarse a bases de datos, manejar la seguridad y controlar la comunicación con el Front-end. Aquí se utilizan lenguajes y entornos como **PHP**, **Python**, **Java (JSP/Servlets)**, **C# (.NET)**, **Ruby** o **Node.js**.

Actualmente, el Back-end tiende a ser **independiente del tipo de cliente**, lo que se conoce como enfoque “universal” o “agnóstico”. Esto se logra mediante el uso de **APIs (Interfaces de Programación de Aplicaciones)**, que permiten que el servidor envíe y reciba datos en formatos estándar como **JSON** o **XML**. Gracias a esto, un mismo Back-end puede ser utilizado por distintos tipos de clientes: aplicaciones web, móviles, de escritorio o incluso otros sistemas externos. Este modelo facilita la **integración**, la **escalabilidad** y la **reutilización** del código.

![img](images/front-back-api.jpg)

## 3.3 Web vs App Web

Aunque a veces se confunden, una **página web** y una **aplicación web** no son lo mismo.  
Una **página web** es principalmente informativa. Puede ser **estática** (su contenido no cambia salvo que se edite manualmente) o **dinámica** (el contenido se genera en función del usuario o de los datos del servidor). Las páginas estáticas pueden verse sin necesidad de un servidor, mientras que las dinámicas requieren de uno para funcionar correctamente.

Una **aplicación web**, en cambio, es un sistema más complejo que ofrece servicios o funcionalidades interactivas. Se ejecuta dentro de un navegador, pero funciona como una aplicación de escritorio: permite registrar usuarios, gestionar datos o realizar tareas específicas. Siempre necesita un **servidor web** y, normalmente, una **base de datos**.

## 3.3 Arquitecturas web y su impacto en el despliegue

La arquitectura condiciona cómo empaquetar y desplegar la aplicación.


- Monolítica
  - Qué hace: Agrupa interfaz, lógica y datos en una sola aplicación/ejecutable; todas las funcionalidades se encuentran juntas.
  - Cómo se despliega: Empaquetas la aplicación como una unidad (p. ej. WAR/JAR, paquete PHP o una única imagen Docker) y la subes a un servidor/VPS o PaaS; ejecutar el proceso único que sirve toda la app.
  - Ideal para: Proyectos pequeños, MVPs.

- De capas (MVC)
  - Qué hace: Separa responsabilidades en Modelo (datos), Vista (presentación) y Controlador (lógica); facilita organización y pruebas.
  - Cómo se despliega: Se publica la aplicación (puede ser una sola unidad) pero con componentes separados en la infra (ej. servidor web + proceso aplicación + base de datos) o como contenedores independientes que representan cada capa.
  - Ideal para: Aplicaciones empresariales con requisitos claros.

- Microservicios
  - Qué hace: Divide la aplicación en servicios pequeños e independientes, cada uno con una responsabilidad concreta y su propio ciclo de vida.
  - Cómo se despliega: Cada servicio se empaqueta por separado (habitualmente en contenedores Docker) y se despliega con un orquestador (Kubernetes, ECS) o plataformas de contenedores; se usan pipelines CI/CD por servicio, balanceadores y service discovery.
  - Ideal para: Aplicaciones complejas, grandes empresas (ej. Netflix).

- Serverless (FaaS)
  - Qué hace: La funcionalidad se implementa como funciones pequeñas que se ejecutan bajo demanda ante eventos; no gestionas servidores.
  - Cómo se despliega: Subes las funciones al proveedor (AWS Lambda, Azure Functions, Google Cloud Functions, Vercel) y configuras triggers (HTTP, colas, cron); el proveedor gestiona ejecución, escalado y recursos.
  - Ideal para: Funciones esporádicas, microservicios específicos y tareas event-driven.

- SOA / EDA (Servicios y Eventos)
  - Qué hace: Servicios independientes se comunican mediante mensajes o eventos; en EDA, las acciones se disparan por eventos (pub/sub).
  - Cómo se despliega: Despliegas los servicios (contenedores o servicios gestionados) y pones en marcha un broker o bus de eventos (RabbitMQ, Kafka, MQTT) para que los publicadores y consumidores intercambien mensajes; configuras consumidores y manejos de reintentos/orden.
  - Ideal para: Integración entre sistemas heterogéneos y procesamiento asíncrono a gran escala.

![img](images/arquitecturas_web.gif)

## 3.4 Protocolos y comunicación. HTTP

La comunicación entre cliente y servidor y entre servicios internos se basa en protocolos y formatos.

El **Protocolo HTTP (HyperText Transfer Protocol)** constituye la base de la comunicación en la World Wide Web. Es un protocolo **no orientado a la conexión**, lo que significa que cada intercambio entre cliente y servidor es independiente y no requiere mantener una conexión constante.

Sus principales **características** son:
- **Sencillo**: Está basado en texto plano, lo que facilita su comprensión y uso directo por parte de una persona.
- **Extensible**: Permite incluir cabeceras adicionales para enviar más información que la establecida por defecto.
- **Sin estado**: Cada solicitud se procesa de manera aislada. Esto puede representar un inconveniente en aplicaciones como los carritos de compra, pero se soluciona mediante *cookies* y sesiones.

HTTP es esencial en arquitecturas distribuidas como los microservicios y constituye el pilar de las APIs REST.  
Entre sus **ventajas**, destaca el control eficiente de la **caché**, la posibilidad de implementar **autenticación** de usuarios, el uso de **proxies** de manera transparente y el manejo del estado entre peticiones mediante **sesiones**.  
Además, permite especificar el formato de los datos enviados, solicitados o recibidos.

### Formato de Peticiones y Respuestas HTTP

La comunicación en la web se basa en un intercambio constante de **peticiones** y **respuestas HTTP** entre el navegador del cliente y el servidor.

Una **petición HTTP** comienza con una línea inicial que contiene el método (por ejemplo, `GET`), la ruta del recurso (`/index.html`) y la versión del protocolo (`HTTP/1.1`).  
A continuación, se incluyen varias cabeceras con metadatos relevantes.

La **respuesta HTTP** del servidor inicia con la versión del protocolo (`HTTP/1.1`), seguida de un código de estado (por ejemplo, `200 OK`) y una breve descripción del resultado.  
Después de una línea vacía, se incorpora el contenido del recurso solicitado (por ejemplo, el documento HTML).

![img](images/http.png)

### Cabeceras HTTP

Las **cabeceras HTTP** son elementos adicionales que acompañan tanto a las peticiones como a las respuestas y aportan información esencial sobre la comunicación.

#### Cabeceras de Petición Frecuentes:
- `Accept`: Especifica el formato MIME de los datos esperados (por ejemplo, `text/html`, `application/json`).
- `Accept-Language`: Define el idioma preferido para la respuesta (por ejemplo, `fr`).
- `Host`: Indica el dominio al que se dirige la solicitud, útil para servidores que alojan múltiples sitios.
- `Content-Type`: Describe el formato y la codificación de los datos incluidos en el cuerpo de la solicitud.
- `Content-Length`: Indica el tamaño, en bytes, del contenido enviado.
- `User-Agent`: Proporciona información sobre el navegador o cliente que realiza la petición.

#### Cabeceras de Respuesta Habituales:
- `Content-Type`: Especifica el formato y la codificación de los datos devueltos (por ejemplo, `text/html; charset=utf-8`), fundamental para que el navegador los interprete correctamente.
- `Content-Language`: Define el idioma del contenido.
- `Content-Length`: Informa del tamaño del cuerpo de la respuesta.
- `Cache-Control`: Indica el tiempo que los datos pueden permanecer almacenados en caché.
- `Server`: Muestra información sobre el servidor que respondió (por ejemplo, `Apache/2.2.3`).


### Métodos o Verbos HTTP (GET, POST, PUT, DELETE, HEAD)

Los **métodos HTTP** —también conocidos como **verbos**— determinan la acción que el cliente desea ejecutar sobre un recurso del servidor.

- **GET**: Se emplea para **obtener** o consultar un recurso. Generalmente no incluye cuerpo en la solicitud; los parámetros se añaden a la URL como *query string*.
- **POST**: Se utiliza para **enviar** datos al servidor o **crear** un nuevo recurso. Los datos se incluyen en el cuerpo de la petición y no son visibles en la URL.
- **PUT**: Permite **actualizar** o **reemplazar** por completo un recurso existente.
- **DELETE**: Se usa para **eliminar** un recurso determinado del servidor.
- **HEAD**: Solicita únicamente las cabeceras que devolvería un `GET`, sin el cuerpo. Es útil para comprobar la existencia o los metadatos de un recurso sin descargarlo.

![img](images/http-metodos.gif)


### Códigos de Estado HTTP

Cada respuesta del servidor incluye un **código de estado HTTP**, un número de tres cifras que comunica el resultado de la solicitud.

- **1XX (Informativas)**: La petición ha sido recibida y se continúa procesando.
- **2XX (Éxito)**: La solicitud fue recibida, comprendida y procesada correctamente. Ejemplo: **200 OK**.
- **3XX (Redirección)**: Se requiere una acción adicional por parte del cliente (por ejemplo, el recurso ha cambiado de ubicación).
- **4XX (Error del Cliente)**: Indica un problema en la petición (por ejemplo, **403 Forbidden**, **404 Not Found**).
- **5XX (Error del Servidor)**: Señala un fallo interno al intentar procesar una solicitud válida.


### HTTPS (SSL/TLS y Certificados Digitales)

**HTTPS** (HyperText Transfer Protocol Secure) es la versión **segura** de HTTP y resulta imprescindible para el intercambio confidencial de información entre cliente y servidor.  
A diferencia de HTTP, que transmite los datos en texto plano y es susceptible de ser interceptado, HTTPS **cifra** la comunicación para garantizar la privacidad.

La seguridad de HTTPS se basa en el uso de **certificados digitales**, documentos electrónicos que asocian una clave pública con la identidad de su propietario (por ejemplo, un sitio web).  
Estos certificados son emitidos por **Autoridades de Certificación (AC)**, entidades de confianza que los firman digitalmente para validar su autenticidad.  
Los navegadores verifican esta firma y alertan al usuario cuando un certificado es inválido, está autofirmado o no coincide con el dominio, mostrando advertencias de seguridad.


![img](images/apirest.png)

## 3.5 Servicio Web y Comunicación con APIs

## 3.5.1 ¿Qué es un Servicio Web?

Un servicio web es un sistema que permite que dos aplicaciones se comuniquen a través de la web para intercambiar datos o utilizar funcionalidades remotas. Se apoya en protocolos estándar (típicamente HTTP) y en formatos de datos como JSON o XML. Cada funcionalidad ofrecida se expone mediante un endpoint (una URL del servidor).

A menudo se habla de API porque la API es la interfaz que define cómo otras aplicaciones acceden al servicio web (sus endpoints, operaciones y datos). Y se suele decir API REST porque el estilo REST, basado en recursos y operaciones HTTP (GET, POST, PUT, DELETE), se ha convertido en la forma más común de implementar servicios web modernos debido a su simplicidad, compatibilidad y facilidad de uso.

Beneficios principales:
- Integración: conecta sistemas heterogéneos.
- Escalabilidad y flexibilidad: divide funcionalidades en servicios reutilizables.
- Reutilización: un mismo servicio puede consumirse desde web, móvil o escritorio.
- Independencia del cliente: el servidor entrega datos, no vistas.

---

## 3.5.2. Protocolos de Comunicación para APIs

La elección del protocolo o estilo de comunicación de la API es una decisión arquitectónica que afecta rendimiento, escalabilidad, experiencia de usuario y costes. A continuación se presenta una comparación de los protocolos más usados.

### Tabla comparativa: REST, GraphQL, gRPC, WebSocket, SOAP

| Protocolo / Estilo | ¿Qué es? | Formato de datos | Comunicación | Ventaja principal | Desventaja principal | Uso ideal |
|---|---|---:|---|---|---|---|
| REST | Estilo arquitectónico basado en HTTP que expone recursos (URLs) y usa verbos HTTP. | JSON (principal), XML | Peticiones HTTP estándar (GET, POST, PUT, DELETE) | Simple y ampliamente adoptado; fácil de cachear | Puede requerir varias peticiones para datos relacionados (over/under-fetching) | APIs públicas, CRUD, microservicios sencillos |
| GraphQL | Lenguaje de consulta que permite al cliente solicitar exactamente los datos necesarios. | JSON (respuestas) | POST con la consulta en el cuerpo (también puede usar GET en consultas simples) | Evita over/under-fetching; una única petición para datos complejos | Implementación más compleja; caching HTTP tradicional más difícil | Interfaces ricas (SPA, dashboards), clientes móviles con ancho limitado |
| gRPC | Framework RPC de alto rendimiento que usa HTTP/2 y Protocol Buffers (Protobuf). | Protobuf (binario) | HTTP/2 (soporta streaming) | Muy eficiente y de baja latencia; contratos fuertemente tipados | Menos legible; requiere grpc-web o proxies para navegadores | Comunicación interna entre microservicios, sistemas de alta demanda |
| WebSocket | Protocolo bidireccional y persistente para comunicación en tiempo real. | Texto/JSON/Binario | Conexión persistente TCP tras handshake HTTP | Comunicación en tiempo real, full-duplex | Conexiones con estado: escalado y gestión más complejos | Chats, actualizaciones en vivo, colaboración en tiempo real |
| SOAP | Protocolo basado en XML con estándares formales y WSDL para contratos. | XML | Mensajes estructurados (normalmente sobre HTTP POST) | Estándares empresariales para seguridad y transacciones | Verboso y más complejo que JSON/REST | Entornos corporativos, sistemas legacy, transacciones distribuidas |

Resumen rápido:
- REST: simplicidad y compatibilidad.
- GraphQL: flexibilidad en las consultas y reducción de llamadas.
- gRPC: rendimiento y tipado fuerte entre servicios.
- WebSocket: comunicación en tiempo real bidireccional.
- SOAP: contratos formales y estándares empresariales.

![img](images/apis.gif)

### Otros Protocolos y Estilos de Comunicación

Existen otros protocolos y estilos de comunicación que se adaptan a necesidades específicas:

- Webhook: Mecanismo de callback HTTP que permite a un servicio enviar notificaciones en tiempo real a una URL preconfigurada cuando ocurre un evento específico (por ejemplo, un pago confirmado). El receptor expone un endpoint que acepta POSTs y procesa los eventos entrantes.
- MQTT (Message Queuing Telemetry Transport): Protocolo ligero de publicación–suscripción diseñado para dispositivos con recursos limitados y redes de bajo ancho de banda. Es el estándar en IoT por su bajo consumo, modelo pub/sub y tolerancia a conexiones intermitentes.
- Apache Kafka: Plataforma distribuida de streaming de eventos que sirve como columna vertebral para arquitecturas modernas basadas en eventos. Permite publicar, almacenar y procesar grandes volúmenes de mensajes con baja latencia y alta tolerancia a fallos, facilitando el desacoplamiento entre productores y consumidores y el procesamiento en tiempo real.

## 3.6 Tecnologías para el Desarrollo de Servicios

El desarrollo de servicios se centra en la creación de APIs para que las aplicaciones se comuniquen. Las tecnologías y frameworks disponibles varían según el lenguaje y el paradigma de la API (REST, RPC, realtime, etc.). A continuación se muestra una tabla comparativa con tecnologías populares y su uso principal.

| Tecnología | Lenguaje | Uso Principal | Ejemplos de Frameworks / Librerías |
|---:|---|---|---|
| Java con Spring Boot | Java | APIs RESTful, microservicios, aplicaciones web de alta escala y enterprise | Spring MVC, Spring WebFlux, Spring Data REST, Hibernate |
| C# con ASP.NET Core | C# | APIs RESTful, microservicios, servicios en la nube, aplicaciones web empresariales | ASP.NET Core Web API |
| PHP con Laravel | PHP | APIs RESTful, aplicaciones web con MVC, desarrollo rápido | Laravel (API Resources), componentes de Symfony |
| Node.js (JavaScript) | JavaScript | APIs RESTful, microservicios, aplicaciones en tiempo real, full-stack JavaScript | Express.js, NestJS, Hapi.js, Meteor.js |
| Python con Django / Flask | Python | APIs RESTful, aplicaciones web complejas, backend para ML y datos | Django REST Framework, Flask-RESTful |
| Ruby con Rails | Ruby | APIs RESTful, aplicaciones web con MVC, desarrollo rápido | Ruby on Rails (API mode) |

Notas rápidas:
- La elección depende del equipo, del ecosistema y de los requisitos no funcionales (latencia, concurrencia, facilidad de despliegue, integraciones).
- Para APIs públicas y rápidas de poner en marcha, frameworks como Express, Flask o Laravel son habituales.
- Para sistemas de gran escala o enterprise, Spring Boot y ASP.NET Core ofrecen herramientas maduras para seguridad, transacciones y despliegue.


## 3.7 Gestores de Bases de Datos

Los gestores de bases de datos son componentes fundamentales en cualquier plataforma web moderna. Son software encargados de almacenar, estructurar y recuperar grandes volúmenes de datos de manera eficiente.

- MySQL / MariaDB  
  Gestores de bases de datos relacionales de código abierto, muy populares por su eficiencia y velocidad. Se usan frecuentemente junto a PHP y otras pilas LAMP. MariaDB es un fork de MySQL totalmente libre.

- PostgreSQL  
  Gestor relacional de código abierto conocido por su robustez, cumplimiento de estándares y por ofrecer funcionalidades avanzadas para consultas complejas y control de integridad.

- SQL Server  
  Sistema gestor de bases de datos de Microsoft, habitual en entornos empresariales Windows (WISA). Ofrece buena integración con el ecosistema Microsoft y herramientas empresariales.

- MongoDB  
  Base de datos NoSQL orientada a documentos. Es flexible en el esquema, escalable y adecuada para datos semi-estructurados o aplicaciones que requieren cambios frecuentes en el modelo de datos.



## 3.8 Ejemplo de arquitectura y tecnologias

Netflix utiliza una arquitectura orientada a **microservicios** que les permite separar su plataforma en servicios pequeños e independientes. Esto mejora la escalabilidad, la disponibilidad y la fiabilidad de la plataforma.

* **Microservicios**: Cada función, como la autenticación de usuarios, la gestión de perfiles o las recomendaciones de contenido, se ejecuta como un servicio individual. Esto permite que los equipos de desarrollo trabajen y desplieguen servicios de forma independiente.
* **Basado en la nube**: Netflix migró completamente a **Amazon Web Services (AWS)**, lo que le permite escalar recursos de forma dinámica según la demanda.
* **Open Connect**: Es su propia red de distribución de contenido (CDN) global. Este sistema almacena copias del contenido de Netflix en servidores locales de los proveedores de servicios de internet (ISP) para acercar el contenido a los usuarios, reduciendo la latencia y la carga en la red.
  **Tecnologías Front-end**

El front-end de Netflix es la interfaz de usuario con la que los suscriptores interactúan, y se centra en una experiencia de usuario (UX) fluida y personalizada.

* **Lenguajes y frameworks**:
  * **JavaScript y React**: El sitio web y las aplicaciones para Smart TVs utilizan JavaScript, en su mayoría con la biblioteca **React**, para construir componentes y páginas de manera eficiente.
  * **HTML y CSS**: Son la base para la estructura y el diseño visual de la interfaz.

**Tecnologías Back-end**

El back-end es el cerebro de la plataforma, encargado de la lógica del negocio, el manejo de datos y el procesamiento de contenido.

* **Lenguajes de programación**:
  * **Java**: Es el lenguaje principal para la mayoría de los microservicios. Su rendimiento, escalabilidad y robustez lo hacen ideal para el núcleo de la arquitectura.
  * **Python**: Se utiliza para tareas específicas, como el análisis de datos, el aprendizaje automático para el sistema de recomendaciones y la automatización de procesos.
* **Bases de datos**:
  * **Apache Cassandra (NoSQL)**: Se usa para manejar grandes volúmenes de datos distribuidos, como la actividad de visualización de los usuarios, que requiere alta disponibilidad y un rendimiento de lectura/escritura muy rápido.
  * **MySQL**: Se utiliza para datos que requieren la consistencia de una base de datos relacional, como la facturación y los pagos.
* **APIs**: La comunicación entre los servicios y el front-end se realiza a través de **APIs (Interfaces de Programación de Aplicaciones)** que permiten un intercambio de datos constante y eficiente.
* **Codificación de video**: Cuando el contenido es subido, el back-end lo procesa y lo codifica en múltiples formatos y resoluciones (4K, HD, etc.) para que pueda ser entregado a cualquier dispositivo, en cualquier calidad de conexión.


![img](images/netflix.gif)


## 3.9 Servidor Web y Servidor de Aplicaciones

### Servidor Web
Un servidor web es un software que recibe peticiones HTTP de los navegadores y devuelve los recursos solicitados. Normalmente entrega archivos estáticos, como páginas HTML, imágenes, hojas de estilo o scripts.

Además, un servidor web puede enviar ciertas peticiones a otros sistemas cuando es necesario generar contenido dinámico.

Ejemplos: Apache HTTP Server, Nginx.

Funciones principales de un servidor web:
- Atender peticiones HTTP.
- Servir contenido estático.
- Gestionar dominios y sitios mediante Virtual Hosts.
- Aplicar reglas de seguridad, compresión o redirecciones.
- Enviar peticiones a aplicaciones externas cuando se necesita contenido dinámico.

### Servidor de Aplicaciones
Un servidor de aplicaciones es un software que ejecuta la lógica de una aplicación. No solo entrega archivos, sino que procesa código, realiza operaciones, consulta bases de datos y genera respuestas dinámicas según la petición del usuario.

Estos servidores ofrecen servicios adicionales como gestión de sesiones, seguridad o comunicación con bases de datos.

Ejemplo: Apache Tomcat (para aplicaciones Java con servlets y JSP).

Funciones principales de un servidor de aplicaciones:
- Ejecutar código del lado del servidor.
- Generar contenido dinámico.
- Gestionar sesiones y autenticación.
- Establecer conexiones con bases de datos.
- Proporcionar un entorno de ejecución para aplicaciones de distintos lenguajes o frameworks.



--------------------
EN CONSTRUCCIÓN!!! EL RESTO PENDIENTE DE COMPLETAR LA INFORMACIÓN
---------------------------------------







## 3.6 Plataformas y entornos de ejecución

Tipos y recomendaciones:

- VPS / Servidor Dedicado:
  - Control total, ideal para aprendizaje o infra pequeña.
  - Requiere gestión de sistema operativo, seguridad y backups.

- PaaS:
  - Abstracción de infra, despliegue por push o docker.
  - Ejemplos: Heroku, Render, Railway, Google App Engine.
  - Rápido para prototipos y MVPs.

- Contenedores (Docker):
  - Empaqueta app y dependencias.
  - Reproducible entre entornos.

- Orquestación (Kubernetes):
  - Gestiona despliegues, escalado, servicios, configuraciones.
  - Añade curva de aprendizaje, pero escala bien.
  - Alternativas: Docker Swarm, AWS ECS.

- Serverless / FaaS:
  - Despliegue de funciones (AWS Lambda, Vercel, Netlify).
  - Excelentes para endpoints pequeños, tareas temporales y reducción de coste operacional.

---
## 3.8 Estrategias y patrones de despliegue

Formas de publicar nuevas versiones:

- Manual:
  - FTP/SSH, poco recomendable fuera de emergencias.
- Automatizado (CI/CD):
  - Pipelines que compilan, testean y despliegan.
  - Herramientas: GitHub Actions, GitLab CI, Jenkins, CircleCI.

Patrones de despliegue:
- Blue-Green: dos entornos, se cambia tráfico al entorno nuevo tras validación.
- Rolling Update: actualizar por lotes sin downtime completo.
- Canary Release: exponer la nueva versión a un % de usuarios y monitorizar.
- Immutable Deployments: crear nueva infraestructura y sustituirla, en lugar de mutar la existente.

Rollback:
- Mantener artefactos versionados.
- Scripts de migración reversibles.
- Estrategias automáticas en pipelines para revertir si fallan health checks.

---

## 3.9 Seguridad y monitorización

Seguridad
- Forzar HTTPS.
- Gestión de secretos: usar vaults (AWS Secrets Manager, HashiCorp Vault) o variables de entorno en CI.
- Autenticación/Autorización: OAuth2, JWT, sesiones seguras.
- CORS: permitir solo orígenes necesarios.
- Validación y saneamiento de entradas para prevenir XSS, SQLi, CSRF.
- Actualizaciones y parches de dependencias: usar escaneo de vulnerabilidades (Dependabot, Snyk).

Monitorización y observabilidad
- Logs estructurados (JSON) y centralizados (ELK, EFK).
- Métricas: Prometheus + Grafana, Datadog.
- Alertas: configurar umbrales y notificaciones (Slack, e-mail, PagerDuty).
- Health checks y readiness probes (Kubernetes).
- Backups: programados, probados y restaurados periódicamente.
- Auditoría: registro de cambios críticos y accesos.

---

## Licencia de uso

Este contenido puede incluir material con licencia **Creative Commons**. Si desea usar, compartir o modificar este material para fines docentes o formativos cite al autor y mantenga condiciones de uso. Ver: https://joseluisgs.dev/docs/license/

### Autor
En construcción, falta detalla mucho y añadir ejercicios para el tema.
Repositorio creado por [CGARCHER](https://github.com/CGARCHER).
---


