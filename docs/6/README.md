# Verificación y Validación - UT6

## 6. Contenido

- [6.1. Introducción](#61-introducción)
- [6.2. ¿Qué significa realmente "calidad" en el código?](#62-qué-significa-realmente-calidad-en-el-código)
  - [6.2.1. Fiabilidad](#621-fiabilidad)
  - [6.2.2. Robustez](#622-robustez)
  - [6.2.3. Mantenibilidad](#623-mantenibilidad)
  - [6.2.4. Seguridad](#624-seguridad)
  - [6.2.5. Factores que arruinan la calidad](#625-factores-que-arruinan-la-calidad)
- [6.3. Verificación y Validación (V&V): ¿El trabajo cumplió el objetivo?](#63-verificación-y-validación-vv-el-trabajo-cumplió-el-objetivo)
  - [6.3.1. ¿Qué es Verificación?](#631-qué-es-verificación)
  - [6.3.2. ¿Qué es Validación?](#632-qué-es-validación)
- [6.4. Tipos de Pruebas en QA: ¡Sigue la Pirámide!](#64-tipos-de-pruebas-en-qa-sigue-la-pirámide)
  - [6.4.1. Pruebas Unitarias](#641-pruebas-unitarias)
  - [6.4.2. Pruebas de Integración](#642-pruebas-de-integración)
  - [6.4.3. Pruebas E2E (End-to-End)](#643-pruebas-e2e-end-to-end)
- [6.5. Herramientas Fundamentales: El Kit de un Desarrollador Profesional](#65-herramientas-fundamentales-el-kit-de-un-desarrollador-profesional)
  - [6.5.1. Análisis de Calidad: SonarQube](#651-análisis-de-calidad-sonarqube)
  - [6.5.2. Linters: ESLint/Checkstyle/PMD](#652-linters-eslintcheckstylepmd)
  - [6.5.3. Pruebas de Cobertura: Jacoco (para Java)](#653-pruebas-de-cobertura-jacoco-para-java)
- [6.6. Automatización: QA en el CI/CD](#66-automatización-qa-en-el-cicd)
- [6.7. Seguridad = Calidad](#67-seguridad--calidad)
  - [6.7.1. Análisis de seguridad en aplicaciones](#671-análisis-de-seguridad-en-aplicaciones)
  - [6.7.2. Revisión de dependencias](#672-revisión-de-dependencias)
  - [6.7.3. Gestión de secretos y credenciales](#673-gestión-de-secretos-y-credenciales)
- [6.8. Coste de detección en fases del desarrollo](#68-coste-de-detección-en-fases-del-desarrollo)
- [6.9. Conclusión: QA para Profesionales del Futuro](#69-conclusión-qa-para-profesionales-del-futuro)

## 6.1. Introducción

Como futuros desarrolladores, vais a construir el software que millones de personas podrían usar. Pero programar no se trata solo de hacer que “funcione”. **La calidad importa.** ¿Por qué algunos proyectos escalan sin problemas y otros están llenos de errores?

Todo se reduce a incorporar una buena cultura de desarrollo profesional basada en:
- **Herramientas de análisis de código**: que buscan errores temprano.
- **Pruebas automáticas**: que aseguran que tu app sigue funcionando después de cada cambio.
- **Automatización de despliegues (CI/CD)**: para entregar software rápido y fiable.

## 6.2. ¿Qué significa realmente "calidad" en el código?

La calidad en el desarrollo de software se refleja en varias dimensiones, entre ellas:

### 6.2.1. Fiabilidad
- El sistema funciona correctamente bajo diferentes condiciones (fallos HTTP, carga de muchas peticiones, etc.).
- Herramientas como **Postman**, **JMeter** y **k6** ayudan a realizar pruebas de rendimiento.
- Asegúrate de que tu aplicación tenga funciones para manejar excepciones y responder adecuadamente ante errores imprevistos.

### 6.2.2. Robustez
- El código maneja bien entradas problemáticas, errores del usuario y responde graciosamente a condiciones anómalas.
- Ejemplo: Evitar caídas del sistema si el usuario ingresa datos inesperados o mal formateados.
- Implementar validaciones de entrada estrictas.

### 6.2.3. Mantenibilidad
- El código debe ser **legible** por tus compañeros. Nada es peor que mirar un fragmento y no entender nada porque no sigue estándares.
- Emplea principios como **KISS (Keep It Simple, Stupid)**, **DRY (Don't Repeat Yourself)** y **YAGNI (You Aren't Gonna Need It)**, que simplifican la arquitectura.

> Siempre escribe **código orientado a humanos**, no solo para máquinas. ¡Piensa que alguien más tendrá que leer tu programa en el futuro!

### 6.2.4. Seguridad
- Un código vulnerable pone en riesgo a los usuarios y a tu organización. Asegura dependencias y autentica cualquier acceso a tu sistema.
- Cifra contraseñas y usa conexiones HTTPS.
- Herramientas como **OWASP Dependency-Check** o **Snyk** ayudan a evaluar la fiabilidad de las bibliotecas externas.

### 6.2.5. Factores que arruinan la calidad:
1. **Líneas duplicadas**: Dificultan los cambios y pueden propagar bugs sin darte cuenta.
2. **Funcionalidad sin testear**: Solo porque “funciona” ahora, no significa que pase todas las pruebas importantes.
3. **Falta de documentación y estándares comunes**: Sin una guía clara, los equipos divergen en enfoques, generando caos.

En definitiva, **calidad** significa más que entregar algo que funcione; significa entregar un producto que sea eficaz, eficiente y sostenible a largo plazo.


## 6.3. Verificación y Validación (V&V): ¿El trabajo cumplió el objetivo?

### 6.3.1. ¿Qué es Verificación?
Es un proceso técnico que asegura que:
- El software cumple los requerimientos y especificaciones acordadas al detalle.
- Cada uno de sus módulos funciona de manera aislada correctamente según se diseñó.

Por ejemplo: Si tu especificación técnica dice que tu API debe devolver resultados en formato JSON, la verificación confirmará que esta condición se cumpla siempre.

### 6.3.2. ¿Qué es Validación?
Es asegurarse de que el producto cumple los propósitos para los que fue diseñado y satisface a los usuarios finales.

Ejemplo: ¿Es fácil de usar? ¿Las funcionalidades implementadas resuelven las necesidades originales del cliente?

### Diferencia fundamental:
- **Verificación:** Comprueba si construiste el software como se especificó.
- **Validación:** Comprueba si el software resulta útil y satisfactorio para sus usuarios.


## 6.4. Tipos de Pruebas en QA: ¡Sigue la Pirámide!

### 6.4.1. Pruebas Unitarias
- Se enfocan en los componentes más pequeños de tu programa (como funciones o métodos).
- **Ventajas**: Son rápidas, otorgan confianza inicial y detectan errores en una etapa temprana del desarrollo.

#### Herramientas sugeridas:
- **JUnit** (Java), **Mocha/Jest** (JavaScript/Node.js), **Pytest** (Python), **XUnit** (.NET).
- Automatiza estas pruebas en tus pipelines de integración continua (CI).


### 6.4.2. Pruebas de Integración
Las pruebas de integración aseguran que **módulos distintos funcionan bien juntos**.

#### Ejemplo:
Una API que recibe datos desde un formulario HTML y los almacena en tu base de datos. Las pruebas de integración verifican que esta cadena fluya correctamente.

#### Herramientas Recomendadas:
- **Supertest** para APIs en Node.js.
- **Spring Testing** para Java y frameworks como Spring Boot.
- **Testcontainers**: Ejecuta bases de datos reales dentro de entornos Docker para pruebas más completas.


### 6.4.3. Pruebas E2E (End-to-End)
- Simulan el flujo completo de la aplicación desde el inicio hasta el final.
- Son útiles para evaluar escenarios como el registro de usuarios, inicio de sesión o procesos de compra.

#### Herramientas recomendadas:
- **Selenium**, **Playwright**, **Cypress**: Ayudan a automatizar pruebas del comportamiento del sistema en navegadores web.

#### Desventajas:
- Por lo general, estas pruebas son más lentas comparadas con las unitarias o de integración.
- 💡 **Briconsejo:** Inclúyelas para probar funciones críticas, pero no dependas exclusivamente de ellas.


## 6.5. Herramientas Fundamentales: El Kit de un Desarrollador Profesional

### 6.5.1. Análisis de Calidad: SonarQube
Una herramienta para monitorear continuamente la calidad de tu código. Beneficios:
- Detecta duplicados, vulnerabilidades y otras métricas clave.
- Se puede integrar con pipelines de CI para mantener un estándar en cada commit.

### 6.5.2. Linters: ESLint/Checkstyle/PMD
Ayudan a mantener estándares de estilo consistente en tu proyecto. Las reglas bien configuradas previenen variaciones y errores comunes.

### 6.5.3. Pruebas de Cobertura: Jacoco (para Java)
Cobertura es el porcentaje de tu programa que es ejecutado por pruebas automáticas. Herramientas como **Jacoco** ayudan a identificar qué partes del código necesitan más pruebas.

## 6.6. Automatización: QA en el CI/CD

### ¿Qué es CI/CD?
Es un proceso que asegura que todo pase automáticamente cuando haces cambios en el código:
1. **Continuous Integration (CI):** Cada commit lanza análisis y pruebas automáticas.
2. **Continuous Delivery/Deployment (CD):** Tu código probado automáticamente llega a staging o producción.


## 6.7. Seguridad = Calidad

**No puedes entregar software de calidad que sea inseguro.**

### 6.7.1. Análisis de seguridad en aplicaciones
1. Usa herramientas de análisis de seguridad como **OWASP ZAP** para detectar vulnerabilidades en tu aplicación web.
2. Integra estas herramientas en tu pipeline de CI/CD para ejecutar análisis automáticos en cada cambio relevante.
3. Revisa y corrige los hallazgos de forma periódica, priorizando las vulnerabilidades críticas y altas.

### 6.7.2. Revisión de dependencias
Revisa dependencias con comandos como:
- `npm audit` para Node.js.
- **OWASP Dependency-Check** para compilaciones en Java.
- Servicios como **Snyk**, **Dependabot** (GitHub) o similares.

### 6.7.3. Gestión de secretos y credenciales
Nunca subas credenciales, claves privadas o contraseñas en tu repositorio. Utiliza:
- Archivos `.env` y librerías como **dotenv**.
- Secretos en plataformas como GitHub o GitLab (secrets management).
- Variables de entorno configuradas en el servidor o en el proveedor cloud.

## 6.8. Coste de detección en fases del desarrollo

<div align="center">

| **Fase de Detección** | **Coste Relativo** | **Tiempo de Corrección** |
|-----------------------|--------------------|--------------------------|
| Local                 | 1x                | Minutos                  |
| Testing/Dev           | 10x               | Horas                    |
| Preproducción         | 50x               | Días                     |
| Producción            | 100x-1000x        | Semanas + Daño reputacional |

</div>

💡 **Briconsejo:** Un bug detectado en producción no solo requiere tiempo de desarrollo para corregirse, sino que involucra reuniones de crisis, análisis de impacto, comunicación con clientes afectados, posible compensación y pérdida de confianza. Por eso, invertir en testing es siempre rentable.


## 6.9. Conclusión: QA para Profesionales del Futuro

Tu camino como desarrollador no termina al entregar un proyecto que funcione. A medida que crezcas profesionalmente, será imprescindible:
- Usar buenas prácticas y automatización.
- Colaborar con herramientas que aseguran la calidad.
- Pensar siempre en los usuarios finales.

**La calidad no es negociable. Recuerda: El software se mantiene mejor cuando se entrega con controles de calidad desde el principio. ¡Empieza a trabajar con esta mentalidad desde ahora!**