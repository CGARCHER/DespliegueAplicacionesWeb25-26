# Despliegue de Aplicaciones Web - UT1. Control de Versiones y Documentación

En el cambiante panorama del desarrollo de software, existen dos fundamentos que resultan imprescindibles para garantizar proyectos eficientes, colaborativos y de calidad: el control de versiones y la documentación del código. A lo largo de este tema analizaremos cómo herramientas como Git y GitHub han transformado la manera en que los equipos organizan y supervisan sus proyectos, así como la importancia de una documentación clara y estandarizada para asegurar la mantenibilidad y el éxito a largo plazo de cualquier aplicación.
## 📚 Contenido

1. **Control de Versiones con Git y GitHub**
    - 1.1. Introducción a Git
    - 1.2. Conceptos Clave en Git
    - 1.3. Ciclo de Vida de los Archivos en Git
    - 1.4. Comandos Git Esenciales con Ejemplos Prácticos
    - 1.5. GitHub para la Colaboración
    - 1.6. Flujos de Trabajo Comunes en Git
    - 1.7. Herramientas Complementarias para Git y GitHub

2. **GitHub y Bitbucket: Gestión de Proyectos y Flujo de Trabajo Profesional**
    - 2.1. Gestión de Issues (Tareas, Historias y Bugs)
    - 2.2. Tableros (Boards) de Proyecto
    - 2.3. Documentación con Wikis
    - 2.4. Confluence: El Espacio de Trabajo Centralizado
    - 2.5. Flujo de Trabajo Profesional: Del Commit a la Tarea (Issue)

3. **Documentación de Código**
    - 3.1. Markdown
    - 3.2. JavaDoc (Java)

---

## 1. Control de Versiones con Git y GitHub

El control de versiones es, en esencia, un sistema que registra los cambios realizados en un archivo o un conjunto de archivos a lo largo del tiempo. Su propósito principal es permitir a los desarrolladores recuperar versiones específicas de sus proyectos en cualquier momento, lo que resulta invaluable para rastrear la evolución del software, revertir errores y, fundamentalmente, coordinar el trabajo de múltiples personas en archivos compartidos.

### 1.1. Introducción a Git

Git es un software de control de versiones distribuido, diseñado por Linus Torvalds en 2005, inicialmente para gestionar el desarrollo del núcleo Linux. A diferencia de los sistemas centralizados (como Subversion), Git otorga a cada desarrollador una copia local completa del historial de desarrollo del proyecto. Esto significa que los cambios se propagan entre repositorios locales, y cada desarrollador puede trabajar de forma independiente, incluso sin conexión a la red. Esta naturaleza distribuida fomenta el desarrollo no lineal y agiliza la gestión de ramas y la fusión de diferentes versiones del código.

### 1.2. Conceptos Clave en Git

- **Repositorio (Repository):** Es el corazón del proyecto. Un repositorio es el lugar donde se almacenan todos los datos actualizados y el historial completo de cambios de un proyecto. Se puede concebir como una base de datos o un sistema de archivos en un disco duro que contiene todas las versiones previas a un cambio determinado, incluyendo ramas y etiquetas.

- **Commit (Confirmación):** Un commit es un punto de control en el proceso de desarrollo. Es una "instantánea" de tu proyecto en un momento específico. Cada commit está identificado de forma única por un código hash SHA-1 de 40 caracteres. Los commits también incluyen metadatos como el autor, la fecha y un mensaje descriptivo que explica los cambios realizados.

- **Rama (Branch):** Las ramas representan líneas de desarrollo independientes. La ramificación es una de las características más potentes de Git, ya que te permite trabajar en nuevas funcionalidades o corregir errores sin afectar la base de código principal (comúnmente llamada master o main).

- **Área de Preparación (Staging Area / Index):** Esta es una zona intermedia donde se seleccionan los cambios que se incluirán en el próximo commit. Esto permite construir la instantánea del commit de forma precisa, eligiendo exactamente qué cambios se guardarán.

- **Directorio de Trabajo (Working Directory):** Es la copia de los archivos del proyecto que tienes en tu máquina local. Aquí es donde realizas las modificaciones directamente.

### 1.3. Ciclo de Vida de los Archivos en Git

El flujo de trabajo básico en Git sigue una secuencia lógica:

1. **Modificar archivos:** Realizas cambios en uno o varios archivos dentro de tu directorio de trabajo.
2. **Preparar archivos:** Añades los archivos modificados al área de preparación. Esto le indica a Git qué cambios específicos deseas incluir en la próxima instantánea.
3. **Confirmar cambios:** Realizas un commit, lo que toma los archivos tal como están en el área de preparación y almacena esa instantánea de forma permanente en tu directorio de Git (repositorio local).

Los archivos en un proyecto Git pueden encontrarse en tres estados principales:

- **Confirmado (Committed):** Los datos están almacenados de forma segura en tu base de datos local (directorio Git).
- **Modificado (Modified):** Has modificado un archivo en tu directorio de trabajo, pero aún no lo has añadido al área de preparación.
- **Preparado (Staged):** Has marcado un archivo modificado en su versión actual para que se incluya en la próxima confirmación.

### 1.4. Comandos Git Esenciales con Ejemplos Prácticos

Dominar Git requiere tiempo, pero algunos comandos son utilizados con mucha frecuencia. Aquí se detallan los más importantes con ejemplos:

#### Configuración Inicial

Antes de empezar, es crucial configurar Git con tu identidad. Estas configuraciones solo necesitan hacerse una vez por máquina.

```bash
git config --global user.name "Tu Nombre"
git config --global user.email "tu.email@ejemplo.com"
```

#### Creación y Clonación de Repositorios

- `git init`: Inicializa un nuevo repositorio Git en el directorio actual.

```bash
mkdir mi-proyecto
cd mi-proyecto
git init
```

- `git clone <URL>`: Descarga una copia idéntica de un repositorio remoto existente.

```bash
git clone https://github.com/usuario/repositorio.git
```

#### Gestión de Cambios (Add, Commit, Status, Diff)

- `git status`: Muestra el estado actual del directorio de trabajo y del área de preparación.
- `git add <archivo>`: Añade los cambios de uno o varios archivos al área de preparación para el siguiente commit.
- `git commit -m "mensaje"`: Registra los cambios preparados en el historial del proyecto.
- `git diff`: Muestra las diferencias entre el directorio de trabajo y el área de preparación.

#### Historial y Deshacer Cambios

- `git log`: Permite explorar las revisiones anteriores de un proyecto.
- `git show <commit>`: Muestra los detalles de un commit específico.
- `git revert <hash_commit>`: Deshace los cambios introducidos por un commit específico creando un nuevo commit inverso.
- `git reset --hard HEAD`: ¡Úsese con extrema precaución! Elimina los cambios realizados en el directorio de trabajo y el área de preparación que aún no se hayan confirmado, volviendo al estado del último commit.

#### Ramificación y Fusión (Branch, Checkout, Merge, Rebase)

- `git branch <nombre_rama>`: Crea una nueva rama local.
- `git checkout <nombre_rama>`: Cambia el directorio de trabajo a la rama especificada.
- `git merge <rama>`: Integra los cambios de una rama especificada en la rama actual.

#### Trabajar con Remotos (Remote, Push, Pull, Fetch)

- `git push <remoto> <rama>`: Sube los commits de tu rama local al repositorio remoto.
- `git pull <remoto> <rama>`: Descarga los cambios del repositorio remoto y los fusiona automáticamente con tu rama local actual.

### 1.5. GitHub para la Colaboración

GitHub es una plataforma de alojamiento de repositorios Git que va más allá de ser un simple servidor. Es un centro de colaboración, gestión de proyectos y una red social para desarrolladores.

- **Pull Requests (Solicitudes de Incorporación de Cambios):** Una Pull Request (PR) es una herramienta de comunicación fundamental en el desarrollo colaborativo. Los desarrolladores las utilizan para proponer sus cambios y solicitar una revisión de código antes de que este se fusione con una rama principal.

- **Fork (Bifurcación):** Un Fork consiste en crear una copia de un repositorio existente en tu propia cuenta de GitHub. Esto es especialmente común en proyectos de código abierto donde no tienes permisos de escritura directos en el repositorio original.

### 1.6. Flujos de Trabajo Comunes en Git

Para coordinar el trabajo de un equipo, es necesario acordar cómo se utilizará Git. A estos acuerdos se les llama flujos de trabajo.

- **GitHub Flow:** Un flujo de trabajo simple y ágil, centrado en ramas de vida corta y despliegues continuos. Se basa en cinco principios:
    1. La rama `main` está siempre lista para producción.
    2. Se crean nuevas ramas para trabajar en funcionalidades.
    3. Se abre una Pull Request para revisión.
    4. Se fusionan los cambios en `main` una vez aprobados.
    5. Los cambios se despliegan de inmediato.

- **GitFlow:** Una metodología más estructurada y compleja, popular para proyectos con entregables y ciclos de desarrollo bien definidos. Utiliza dos ramas de larga vida (`master` y `develop`) y varias ramas temporales para características, lanzamientos y correcciones rápidas.

### 1.7. Herramientas Complementarias para Git y GitHub

Además de la línea de comandos, existen diversas herramientas que mejoran la experiencia de uso de Git y GitHub:

- **Visual Studio Code (VS Code):** Un editor de código muy popular con una fuerte integración con Git.
- **GitKraken:** Considerado por muchos como la mejor herramienta GUI (Graphical User Interface) para manejar Git.
- **GitHub Desktop:** Un cliente gráfico desarrollado por GitHub que simplifica las operaciones comunes de Git con una interfaz intuitiva.

---

## 2. GitHub y Bitbucket: Gestión de Proyectos y Flujo de Trabajo Profesional

Además del control de versiones, GitHub y Bitbucket ofrecen herramientas robustas para gestionar proyectos de manera ágil. El uso de Scrum es un enfoque popular para organizar el trabajo en iteraciones cortas llamadas Sprints.

### 2.1. Gestión de Issues (Tareas, Historias y Bugs)

En Scrum, el trabajo se organiza en un **Product Backlog** que se compone de historias de usuario, tareas y errores. En GitHub y Bitbucket, los **Issues** (Incidencias en Bitbucket) son la herramienta ideal para representar estos elementos. Cada issue es una unidad de trabajo rastreable.

- **Creación de Issues:** Cada historia de usuario ("Como usuario, quiero poder loguearme") o tarea ("Crear la base de datos de usuarios") se convierte en un issue individual.
- **Etiquetas (Labels):** Utiliza etiquetas para clasificar los issues (bug, feature, documentation). Esto permite filtrar y organizar el backlog de forma eficiente.
- **Hitos (Milestones):** Agrupa un conjunto de issues bajo un hito para representar un Sprint (por ejemplo, Sprint 1 - Módulo de Autenticación). Esto ayuda a seguir el progreso del trabajo en cada iteración.

### 2.2. Tableros (Boards) de Proyecto

Tanto GitHub como Bitbucket ofrecen tableros visuales (kanban o scrum) que permiten a los equipos ver el estado del trabajo de un vistazo, facilitando la planificación y el seguimiento diario.

- **GitHub Projects:** Te permite crear un board para tu repositorio. Puedes configurar columnas personalizadas (To Do, In Progress, Code Review, Done) y arrastrar los issues a través de ellas.
- **Bitbucket Boards:** Similar a GitHub, Bitbucket ofrece tableros de Kanban y Scrum. Las tarjetas en el tablero representan los issues y se mueven de una columna a otra para mostrar su estado actual.

### 2.3. Documentación con Wikis

El Wiki es una herramienta esencial para la documentación que no vive dentro del código. Es perfecta para capturar información clave del proyecto, como:

- **Visión del Proyecto:** Un documento de alto nivel que describe el propósito y los objetivos a largo plazo del proyecto.
- **Objetivos del Sprint:** Un resumen de lo que el equipo se compromete a lograr en la iteración actual.
- **Glosario de Términos:** Definiciones de conceptos y abreviaturas específicas del proyecto o la empresa.
- **Guías de Estilo:** Normas de codificación, pautas de diseño y convenciones para el equipo.

### 2.4. Confluence: El Espacio de Trabajo Centralizado

Confluence es una herramienta de colaboración de Atlassian que sirve como un espacio de trabajo centralizado para los equipos. Mientras que el wiki del repositorio es útil para documentación sencilla, Confluence es ideal para crear documentos más estructurados y colaborar en ellos. Su integración con Bitbucket y Jira lo convierte en una opción poderosa en entornos profesionales. Puedes enlazar directamente issues de Bitbucket o Jira y commits de tu repositorio a una página de Confluence.

### 2.5. Flujo de Trabajo Profesional: Del Commit a la Tarea (Issue)

En las empresas de desarrollo, la clave del trabajo en equipo es la trazabilidad. Cada cambio de código (commit) debe estar vinculado a una tarea (issue) para que sea fácil entender qué se hizo, quién lo hizo y por qué. Esta práctica asegura transparencia, simplifica el seguimiento del progreso y facilita la resolución de errores.

#### Asociar Commits a Issues

Es una convención estándar asociar los commits directamente a un issue. Para lograr esto, simplemente incluye el identificador del issue en el mensaje de commit. Esto crea un enlace automático en la plataforma (GitHub/Bitbucket) que conecta el cambio de código con la tarea.

**Sintaxis:** El formato más común es añadir `#` y el número del issue al final del mensaje.

```bash
git commit -m "feat: Añadido formulario de login (#15)"
```

También puedes usar palabras clave como `closes #15`, `fixes #15` o `resolves #15` para cerrar el issue automáticamente al fusionar el commit en la rama principal, lo que simplifica la gestión del tablero de proyectos.

#### Desglose y Estimación de Tareas

Una tarea grande y compleja (issue) es difícil de gestionar. Los equipos de desarrollo profesional las dividen en subtareas más pequeñas y manejables.

- **Desglose del Issue:** Cuando se asigna una historia de usuario (ej. "Como usuario, quiero poder iniciar sesión"), se desglosa en issues más pequeñas (ej. "Crear interfaz del formulario de login", "Implementar validación del lado del cliente", "Conectar el formulario a la API").
- **Estimación:** Se asigna una estimación a cada subtarea para medir el esfuerzo requerido. Esto puede hacerse con puntos de historia (una medida de esfuerzo relativa) o con estimaciones de tiempo (ej. 4 horas, 2 días). La estimación ayuda a los equipos a planificar sus Sprints de manera realista.

#### Documentación y Seguimiento Diario

Un issue no es solo una descripción, es un espacio de comunicación y un registro de trabajo.

- **Comentarios Diarios:** A medida que trabajas en una tarea, es una práctica recomendada comentar el issue al final del día. Anota el progreso, los obstáculos que encontraste o las decisiones que tomaste. Esto permite a tus compañeros y al gestor del proyecto ver el progreso sin tener que preguntar.
- **Actualización del Estado:** Actualiza el issue moviéndolo en el tablero (de To Do a In Progress, por ejemplo). Esta simple acción es crucial para que el equipo sepa el estado actual de todas las tareas.

---

## 3. Documentación de Código

La documentación del código es una parte fundamental y a menudo subestimada del desarrollo de software. Es esencial para el mantenimiento futuro de cualquier proyecto, ya que permite a otros desarrolladores (e incluso a tu yo futuro) entender, utilizar y modificar el código sin necesidad de una inmersión profunda. Una buena documentación reduce la curva de aprendizaje y mejora la calidad general del software.

### 3.1. Markdown

Markdown es un lenguaje de marcado ligero diseñado para dar formato a texto plano. Su principal objetivo es lograr la máxima legibilidad y facilidad de escritura. Es el formato por defecto en plataformas como GitHub para archivos README.md y wikis, y es ampliamente utilizado en documentación técnica, blogs y contenido web debido a su simplicidad y portabilidad.

**Ventajas de usar Markdown:**

- **Facilidad de Lectura y Escritura:** Su sintaxis es muy natural y se asemeja a cómo formatearías texto en un correo electrónico.
- **Versatilidad:** Se puede usar para crear una amplia variedad de documentos, desde notas personales hasta libros y documentación de API.
- **Portabilidad:** Los archivos Markdown son texto plano, lo que significa que pueden ser abiertos y editados con cualquier editor de texto.
- **Popularidad y Soporte:** Es un estándar bien establecido en la comunidad tecnológica.

#### Sintaxis Básica de Markdown

La sintaxis de Markdown es intuitiva. Aquí se muestran los elementos más comunes:

- **Encabezados:** Se utilizan de una a seis almohadillas (`#`) al principio de una línea.
  ```
  # Título Principal (H1)
  ## Sección Importante (H2)
  ```

- **Texto Básico (Negrita, Cursiva, Combinado):**
    - Negrita: `**texto en negrita**` o `__texto en negrita__`.
    - Cursiva: `*texto en cursiva*` o `_texto en cursiva_`.
    - Negrita y cursiva: `***texto en negrita y cursiva***`.

- **Listas:**
    - Listas numeradas: Empieza cada elemento con un número seguido de un punto y un espacio.
    - Listas con viñetas: Empieza cada elemento con un guion (-), un asterisco (*) o un signo más (+) seguido de un espacio.

- **Enlaces:** La sintaxis para un enlace en línea consta del texto del enlace entre corchetes `[ ]` seguido de la URL entre paréntesis `( )`.
  ```
  [Visita Google](https://www.google.com)
  ```

- **Imágenes:** Similar a los enlaces, pero precedido por un signo de exclamación `!`.
  ```
  ![Logo de Ejemplo](/assets/logo.png)
  ```

- **Bloques de Código:**
    - Para código en línea, encierra el texto entre comillas invertidas (`` `código en línea` ``).
    - Para bloques de código multilínea, usa tres comillas invertidas (```) antes y después del bloque. Puedes especificar el lenguaje para el resaltado de sintaxis.
      ```java
      public class HelloWorld {
          public static void main(String[] args) {
              System.out.println("Hola, Mundo!");
          }
      }
      ```

- **Tablas:** Se construyen usando barras verticales (`|`) para las columnas y guiones (-) para el encabezado.
  ```
  | Encabezado 1 | Encabezado 2 |
  |--------------|--------------|
  | Fila 1 Col 1 | Fila 1 Col 2 |
  | Fila 2 Col 1 | Fila 2 Col 2 |
  ```

### 3.2. JavaDoc (Java)

JavaDoc es la utilidad estándar de Java para generar documentación de APIs en formato HTML directamente del código fuente. Los comentarios Javadoc se escriben con la sintaxis especial `/** ... */` e incluyen etiquetas especiales (@) para estructurar la información sobre parámetros, valores de retorno y excepciones.

#### Comentarios y Etiquetas en Javadoc

Los comentarios Javadoc se escriben utilizando una sintaxis especial que comienza con `/**` y termina con `*/`. Deben colocarse inmediatamente antes de la declaración de una clase, interfaz, método o miembro. Dentro de estos comentarios, se utilizan "etiquetas" (tags) especiales, precedidas por el símbolo `@`, para estructurar la información.

**Ejemplo de Clase Documentada:**

```java
/**
 * La clase Producto representa un artículo en un inventario con su nombre y precio.
 *
 * <p>Esta clase es fundamental para la gestión de stock y pedidos.</p>
 *
 * @author Tu Nombre
 * @version 1.0.0
 * @see <a href="https://ejemplo.com/docs/productos">Documentación de la API de Productos</a>
 */
public class Producto {

    private String id;

    /**
     * Obtiene el identificador único del producto.
     *
     * @return El {@code String} que representa el ID del producto.
     */
    public String getId() {
        return id;
    }

    /**
     * Establece el precio del producto.
     *
     * @param precio El nuevo precio del producto (debe ser mayor que cero).
     * @throws IllegalArgumentException Si el precio proporcionado es menor o igual a cero.
     */
    public void setPrecio(double precio) {
        if (precio <= 0) {
            throw new IllegalArgumentException("El precio debe ser mayor que cero.");
        }
        // Lógica de asignación...
    }
}
```

#### Generación de la Documentación

Para generar los archivos HTML, puedes usar la línea de comandos o el IDE:

**Desde la Terminal:**
```bash
javadoc -d docs src/main/java/com/mi-proyecto/*.java
```

**Desde IntelliJ IDEA:**
1. Ve a Tools en la barra de menú.
2. Selecciona Generate Javadoc....
3. En la ventana de diálogo, elige el output directory (ej. docs), el Scope (Project) y cualquier opción adicional como `-windowtitle "Documentación de la API"`.
4. Haz clic en OK.

---