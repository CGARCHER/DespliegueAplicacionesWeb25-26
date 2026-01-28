# Análisis de código con SonarQube en Spring Boot

Guía para integrar SonarQube en un proyecto Spring Boot con Maven.

---

## ¿Qué hace SonarQube?

Analiza tu código Java y detecta:

- Bugs
- Vulnerabilidades de seguridad
- Código duplicado
- Problemas de mantenibilidad  

Además, mide la cobertura de tests (integrándose, por ejemplo, con JaCoCo).

---

## Configurar el proyecto

### 1. Actualizar el `pom.xml`

Añade estas propiedades dentro de la sección `<properties>`:

```xml
<properties>
    <java.version>21</java.version>

    <!-- Configuración SonarQube -->
    <sonar.host.url>http://localhost:9000</sonar.host.url>
    <sonar.projectKey>sftp-app</sonar.projectKey>
    <sonar.projectName>SFTP Application</sonar.projectName>
    <sonar.java.source>21</sonar.java.source>
</properties>
```

Añade el plugin de SonarQube dentro de `<build><plugins>`:

```xml
<!-- Plugin SonarQube -->
<plugin>
    <groupId>org.sonarsource.scanner.maven</groupId>
    <artifactId>sonar-maven-plugin</artifactId>
    <version>4.0.0.4121</version>
</plugin>
```

---

### 2. Añadir SonarQube al `docker-compose.yml`

Añade este servicio a tu `docker-compose.yml`:

```yaml
services:
  # Tus servicios existentes...

  sonarqube:
    image: sonarqube:community
    container_name: sonarqube
    ports:
      - "9000:9000"
    environment:
      - SONAR_ES_BOOTSTRAP_CHECKS_DISABLE=true
    volumes:
      - sonarqube_data:/opt/sonarqube/data
      - sonarqube_extensions:/opt/sonarqube/extensions
      - sonarqube_logs:/opt/sonarqube/logs

volumes:
  # Tus volúmenes existentes...
  sonarqube_data:
  sonarqube_extensions:
  sonarqube_logs:
```

Levanta los servicios:

```bash
docker compose up -d
```

SonarQube tarda unos 30–60 segundos en arrancar. Verifica su estado con:

```bash
docker logs sonarqube
```

Cuando veas el mensaje `SonarQube is operational` ya puedes acceder al panel.

---

## Configurar SonarQube (primera vez)

1. Abre [http://localhost:9000](http://localhost:9000) en tu navegador.
2. Login inicial:
   - Usuario: `admin`
   - Password: `admin`
3. Te pedirá cambiar la contraseña (puedes poner algo sencillo, por ejemplo `admin123`).
4. Ve a: **Administration > Security > Users**.
5. Haz clic en el icono de **llave** del usuario `admin`.
6. Genera un token:

   - **Name**: `sftp-app-token`
   - **Type**: `Global Analysis Token`
   - **Expires**: `No expiration`
   - Haz clic en **Generate**.

7. Copia el token (no podrás verlo de nuevo).

   - Ejemplo de formato:  
     `sqp_a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6q7r8s9t0`

---

## Ejecutar el análisis

Desde la **raíz del proyecto**, ejecuta:

```bash
mvn clean verify sonar:sonar -Dsonar.token=TU_TOKEN_AQUI
```

Cambia `TU_TOKEN_AQUI` por el token que generaste en SonarQube.

¿Qué hace cada fase?

- `clean`: limpia compilaciones anteriores.
- `verify`: compila el proyecto y ejecuta tests/verificaciones.
- `sonar:sonar`: ejecuta el análisis y envía los resultados a SonarQube.

---

## Ver los resultados

Al terminar el análisis verás algo como:

```text
INFO: ANALYSIS SUCCESSFUL
INFO: You can browse http://localhost:9000/dashboard?id=sftp-app
```

Entra a esa URL y verás:

- **Bugs**: Errores en el código.
- **Vulnerabilities**: Problemas de seguridad.
- **Code Smells**: Código mal escrito o difícil de mantener.
- **Duplications**: Código repetido.
- **Coverage**: Porcentaje cubierto por tests (necesita configuración de JaCoCo).

---

## Reglas de SonarQube: personalización y adaptación

SonarQube viene con conjuntos de reglas predefinidas (*Sonar way*), pero **las reglas se pueden adaptar totalmente a tu proyecto**:

- Puedes **activar o desactivar reglas**.
- Puedes **cambiar la severidad** de una regla (por ejemplo, de *Major* a *Minor*).
- Puedes crear **Quality Profiles** diferentes según lenguaje o tipo de proyecto (backend, microservicios, etc.).
- Puedes definir **Quality Gates** propios, con tus umbrales de cobertura, duplicación, etc.

### Dónde se configuran las reglas

En la interfaz de SonarQube:

1. Ve a **Quality Profiles**.
2. Elige el lenguaje (por ejemplo, **Java**).
3. Selecciona un perfil (por ejemplo, `Sonar way`) o crea uno nuevo a partir de él.
4. Dentro del perfil puedes:
   - **Desactivar** reglas que no tengan sentido para tu proyecto.
   - **Cambiar severidades**.
   - **Buscar reglas** por texto (por ejemplo, “naming”, “security”, etc.).

**Consejo:** lo habitual es:

- Copiar el perfil **Sonar way**.
- Crear un perfil propio, por ejemplo: `Java - Company Standard`.
- Ajustar reglas según vuestro estilo y necesidades.
- Asignar ese perfil a tus proyectos Java.

### Reglas por proyecto

Si quieres que tu proyecto use un perfil concreto:

1. Ve a **Projects** y entra en tu proyecto (`sftp-app`).
2. Menú **Project Settings > Quality Profiles**.
3. Asigna el perfil de Java que hayas creado.

De esta forma, cada servicio o módulo puede tener su propio conjunto de reglas si lo necesitas.

---

## Entender los niveles de severidad

### Blocker 🔴

Corregir **de inmediato**. Puede romper la aplicación.

**Ejemplos:**

- Divisiones por cero
- `NullPointerException` obvios
- Recursos sin cerrar (archivos, conexiones)
- Bucles infinitos

```java
// BLOCKER: División por cero
int result = 10 / 0;

// BLOCKER: Archivo sin cerrar
FileInputStream file = new FileInputStream("file.txt");
// falta file.close()
```

**Acción:** Arreglar antes de hacer commit.

---

### Critical 🟠

Corregir **pronto**. Problemas graves de seguridad o lógica.

**Ejemplos:**

- Contraseñas hardcodeadas
- SQL Injection
- Uso incorrecto de `Random` en seguridad
- Dependencias con vulnerabilidades conocidas

```java
// CRITICAL: Contraseña en el código
String password = "admin123";

// CRITICAL: SQL Injection
String query = "SELECT * FROM users WHERE name = '" + userName + "'";
```

**Acción:** Priorizar durante la semana.

---

### Major 🟡

Debería corregirse. Afecta mantenibilidad o puede causar bugs.

**Ejemplos:**

- Métodos muy largos (> 50 líneas)
- Complejidad ciclomática alta
- Parámetros que no se usan
- Variables declaradas pero nunca usadas

```java
// MAJOR: Método muy largo
public void processData(int unused) { // parámetro sin usar
    // 100 líneas de código...
}
```

**Acción:** Refactorizar cuando tengas tiempo (añadir al backlog técnico).

---

### Minor 🔵

Opcional, pero recomendado.

**Ejemplos:**

- Variables con nombres poco claros
- Código duplicado menor
- Imports sin usar
- Falta documentación en clases públicas

```java
// MINOR: Nombre poco claro
int x = 10;  // mejor: int maxAttempts = 10;

// MINOR: Import sin usar
import java.util.ArrayList;
```

**Acción:** Corregir si hay tiempo o si estás tocando esa parte del código.

---

### Info ℹ️

Solo información. Sugerencias de mejora.

**Ejemplos:**

- Sugerencias de estilo
- Optimizaciones menores
- Avisos sobre código `deprecated`

```java
// INFO: Método deprecated
@Deprecated
public void oldMethod() { }
```

**Acción:** Opcional, puedes ir aplicándolas progresivamente.

---

## Cómo priorizar

- **Blocker y Critical**: Resolver antes de hacer **merge/deploy**.
- **Major**: Añadir al **backlog** y planificar su corrección.
- **Minor**: Corregir cuando sobre tiempo o durante refactors.
- **Info**: Solo si mejora claramente el código.

---

## Quality Gate

El *Quality Gate* indica si tu código pasa los estándares mínimos definidos:

- ✅ **Passed**: todo correcto.
- ❌ **Failed**: hay problemas que no cumplen los criterios.

Por defecto suele fallar si:

- Hay **bugs Blocker o Critical nuevos**.
- Hay **vulnerabilidades nuevas**.
- **Código duplicado** > 3%.
- **Cobertura nueva** < 80%.

### Quality Gate personalizable

Al igual que las reglas, el Quality Gate también se puede adaptar:

1. Ve a **Quality Gates**.
2. Usa el `Sonar way` o crea uno nuevo.
3. Ajusta las condiciones, por ejemplo:
   - Cobertura en código nuevo ≥ 70% en lugar de 80%.
   - Duplicación en código nuevo ≤ 5% en lugar de 3%.
   - Número máximo de vulnerabilidades nuevas, etc.
4. Asigna tu Quality Gate a los proyectos relevantes en la pestaña **Projects**.

Esto te permite adaptar la exigencia a la realidad de tu proyecto (por ejemplo, proyectos legacy vs. proyectos nuevos).

---

## SonarLint: análisis en tiempo real en tu IDE

[SonarLint](https://www.sonarlint.org/) es un plugin para tu IDE que analiza el código **mientras escribes**, sin necesidad de hacer commit ni ejecutar Maven. Es como tener SonarQube dentro de IntelliJ IDEA, Eclipse o VS Code.

### ¿Qué hace SonarLint?

- Subraya problemas en el código en tiempo real (como el corrector ortográfico).
- Muestra la explicación del problema y cómo solucionarlo.
- Funciona sin conexión a internet.
- Puede sincronizarse con tu servidor SonarQube para usar las mismas reglas (reglas compartidas de calidad).

---

### Instalar SonarLint en IntelliJ IDEA

1. Ve a **File > Settings > Plugins**.
2. Busca **"SonarLint"**.
3. Haz clic en **Install**.
4. Reinicia IntelliJ IDEA.

### Instalar SonarLint en VS Code

1. Ve a **Extensions** (`Ctrl+Shift+X`).
2. Busca **"SonarLint"**.
3. Haz clic en **Install**.

---

### Usar SonarLint

Una vez instalado, ya funciona automáticamente:

- Los problemas aparecen subrayados en amarillo/rojo según severidad.
- Pasa el ratón sobre el problema para ver la explicación.
- Clic derecho > **SonarLint** para más opciones (por ejemplo, ver detalles).

**Ejemplo en IntelliJ:**

```java
public void loadData() {
    FileInputStream file = new FileInputStream("data.txt"); // ⚠️ Subrayado
    // SonarLint te avisa: "Close this file in a finally block or use try-with-resources"
}
```

---

### Conectar SonarLint con tu servidor SonarQube

Para usar las mismas reglas que tu proyecto en SonarQube:

#### En IntelliJ IDEA

1. Ve a **File > Settings > Tools > SonarLint**.
2. En **SonarQube / SonarCloud Connections**, haz clic en `+`.
3. Selecciona **SonarQube**.
4. **Server URL**: `http://localhost:9000`.
5. Genera un token en SonarQube (como hiciste para Maven).
6. Pega el token.
7. Haz clic en **Next** y selecciona tu proyecto.

#### En VS Code

1. Abre la Command Palette (`Ctrl+Shift+P`).
2. Busca **"SonarLint: Add SonarQube Connection"**.
3. **URL**: `http://localhost:9000`.
4. Introduce tu token.
5. Selecciona el proyecto que corresponda.

---

### Ventajas de usar SonarLint

- Detecta problemas **antes de hacer commit**.
- No necesitas ejecutar el análisis completo con Maven.
- Aprendes buenas prácticas mientras programas.
- Ahorra tiempo en revisiones de código (code reviews).

---

## Diferencias SonarLint vs SonarQube

| Característica         | SonarLint                           | SonarQube                             |
|------------------------|--------------------------------------|---------------------------------------|
| Alcance del análisis   | Archivo que estás editando          | Todo el proyecto                      |
| Necesita servidor      | No                                  | Sí (Docker u otro despliegue)        |
| Ejecución              | En tiempo real en el IDE            | Manual/automática vía Maven/CI       |
| Cobertura de tests     | No la mide                          | Sí, con herramientas como JaCoCo     |
| Duplicación de código  | Limitada                            | Proyecto completo                     |
| Uso típico             | Durante el desarrollo               | Antes de push / en CI/CD             |

**Recomendación:**  
Usa **ambos**:

- **SonarLint** mientras programas.
- **SonarQube** antes de hacer push o en tu pipeline de CI/CD.

---

## Comandos útiles (Docker / SonarQube)

Ver logs de SonarQube:

```bash
docker logs sonarqube -f
```

Reiniciar el servicio de SonarQube:

```bash
docker compose restart sonarqube
```

---

## Resumen

1. Configura SonarQube en `pom.xml` y en `docker-compose.yml`.
2. Levanta SonarQube con Docker y genera un token.
3. Ejecuta `mvn clean verify sonar:sonar -Dsonar.token=...`.
4. Revisa el dashboard en `http://localhost:9000`.
5. **Adapta las reglas y el Quality Gate** a la realidad de tu proyecto.
6. Instala SonarLint en tu IDE para detectar problemas en tiempo real.
7. Usa el *Quality Gate* como criterio mínimo de calidad antes de desplegar.

Este flujo te ayudará a mantener un código más limpio, seguro y alineado con las reglas de calidad que mejor encajan con tu equipo y tu proyecto Spring Boot.