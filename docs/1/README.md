# Despliegue de Aplicaciones Web - UT1
## Control de Versiones y Documentación

---

En el cambiante panorama del desarrollo de software, existen dos fundamentos que resultan imprescindibles para garantizar proyectos eficientes, colaborativos y de calidad: el **control de versiones** y la **documentación del código**.  
A lo largo de este tema analizaremos cómo herramientas como **Git** y **GitHub** han transformado la manera en que los equipos organizan y supervisan sus proyectos, así como la importancia de una documentación clara y estandarizada para asegurar la mantenibilidad y el éxito a largo plazo de cualquier aplicación.

---

## 📚 Contenido

- [1. Git: El Sistema de Control de Versiones](#1-git-el-sistema-de-control-de-versiones)
    - [1.1. Introducción a Git](#11-introducción-a-git)
    - [1.2. Conceptos Clave en Git](#12-conceptos-clave-en-git)
    - [1.3. Ciclo de Vida de los Archivos en Git](#13-ciclo-de-vida-de-los-archivos-en-git)
    - [1.4. Comandos Git Esenciales con Ejemplos Prácticos](#14-comandos-git-esenciales-con-ejemplos-prácticos)
- [2. Plataformas de Colaboración: GitHub y Bitbucket](#2-plataformas-de-colaboración-github-y-bitbucket)
    - [2.1. Colaboración en la Nube](#21-colaboración-en-la-nube)
    - [2.2. Flujos de Trabajo](#22-flujos-de-trabajo)
    - [2.3. Gestión de Proyectos con Scrum](#23-gestión-de-proyectos-con-scrum)
    - [2.4. Flujo de Trabajo Profesional: Del Commit a la Tarea (Issue)](#24-flujo-de-trabajo-profesional-del-commit-a-la-tarea-issue)
    - [2.5. Herramientas Complementarias](#25-herramientas-complementarias)
- [3. Documentación de Código](#3-documentación-de-código)
    - [3.1. Markdown](#31-markdown)
    - [3.2. JavaDoc (Java)](#32-javadoc-java)

---

## 1. Git: El Sistema de Control de Versiones

El control de versiones es, en esencia, un sistema que registra los cambios realizados en un archivo o un conjunto de archivos a lo largo del tiempo.  
Su propósito principal es permitir a los desarrolladores recuperar versiones específicas de sus proyectos en cualquier momento, lo que resulta invaluable para rastrear la evolución del software, revertir errores y, fundamentalmente, coordinar el trabajo de múltiples personas en archivos compartidos.

### 1.1. Introducción a Git

Git es un software de control de versiones distribuido, diseñado por Linus Torvalds en 2005, inicialmente para gestionar el desarrollo del núcleo Linux.  
A diferencia de los sistemas centralizados (como Subversion), Git otorga a cada desarrollador una copia local completa del historial de desarrollo del proyecto.  
Esto significa que los cambios se propagan entre repositorios locales, y cada desarrollador puede trabajar de forma independiente, incluso sin conexión a la red.  
Esta naturaleza distribuida fomenta el desarrollo no lineal y agiliza la gestión de ramas y la fusión de diferentes versiones del código.

### 1.2. Conceptos Clave en Git

- **Repositorio (Repository):** Es el corazón del proyecto. Un repositorio es el lugar donde se almacenan todos los datos actualizados y el historial completo de cambios de un proyecto. Incluye ramas y etiquetas.
- **Commit (Confirmación):** Un commit es un punto de control en el proceso de desarrollo. Es una "instantánea" de tu proyecto en un momento específico. Cada commit está identificado de forma única por un código hash SHA-1 de 40 caracteres. Los commits también incluyen metadatos como el autor, la fecha y un mensaje descriptivo que explica los cambios realizados.
- **Rama (Branch):** Las ramas representan líneas de desarrollo independientes. Permiten trabajar en nuevas funcionalidades o corregir errores sin afectar la base de código principal (`main` o `master`).
- **Área de Preparación (Staging Area / Index):** Zona intermedia donde se seleccionan los cambios que se incluirán en el próximo commit.
- **Directorio de Trabajo (Working Directory):** Es la copia de los archivos del proyecto que tienes en tu máquina local, donde realizas las modificaciones.

### 1.3. Ciclo de Vida de los Archivos en Git

El flujo de trabajo básico en Git sigue una secuencia lógica:

1. **Modificar archivos:** Realizas cambios en uno o varios archivos dentro de tu directorio de trabajo.
2. **Preparar archivos (staging):** Añades los archivos modificados al área de preparación (*staging area*). Esto le indica a Git qué cambios específicos deseas incluir en la próxima instantánea.
3. **Confirmar cambios (commit):** Creas una confirmación que toma los archivos del área de preparación y guarda esa instantánea de forma permanente en tu repositorio local.

#### Estados principales de los archivos

- **Modificado (Modified):** El archivo ha cambiado en el directorio de trabajo, pero aún no se ha añadido al área de preparación.
- **Preparado (Staged):** El archivo modificado ha sido marcado para incluirse en la próxima confirmación.
- **Confirmado (Committed):** Los cambios ya se guardaron de forma segura en la base de datos del repositorio local.

### 1.4. Comandos Git Esenciales con Ejemplos Prácticos

#### Configuración Inicial

```bash
git config --global user.name "Tu Nombre"
git config --global user.email "tu.email@ejemplo.com"
```

#### Creación de Repositorios

```bash
mkdir mi-proyecto
cd mi-proyecto
git init
```

#### Gestión de Cambios (Add, Commit, Status, Diff)

```bash
git status              # Muestra el estado actual del directorio de trabajo y del área de preparación.
git add <archivo>       # Añade los cambios al área de preparación para el siguiente commit.
git commit -m "mensaje" # Registra los cambios preparados en el historial del proyecto.
git diff                # Muestra las diferencias entre el directorio de trabajo y el área de preparación.
```

#### Historial y Deshacer Cambios

```bash
git log                 # Permite explorar las revisiones anteriores de un proyecto.
git show <commit>       # Muestra los detalles de un commit específico.
git revert <hash>       # Deshace los cambios de un commit específico (creando uno inverso).
git reset --hard HEAD   # Elimina los cambios no confirmados y vuelve al último commit.
```

#### Ramificación y Fusión (Branch, Checkout, Merge, Rebase)

```bash
git branch <nombre_rama>      # Crea una nueva rama local.
git checkout <nombre_rama>    # Cambia al directorio de trabajo de la rama especificada.
git merge <rama>              # Integra los cambios de una rama en la actual.
```

---

## 2. Plataformas de Colaboración: GitHub y Bitbucket

Una vez que un proyecto está bajo control de versiones con Git, se necesitan plataformas como GitHub o Bitbucket para la colaboración en equipo.  
Estas plataformas sirven como repositorios remotos que alojan el código, permitiendo la sincronización y la gestión de proyectos entre múltiples desarrolladores.

### 2.1. Colaboración en la Nube

```bash
git clone <URL>                # Clona un repositorio remoto.
git push <remoto> <rama>       # Sube los commits locales al repositorio remoto.
git pull <remoto> <rama>       # Descarga y fusiona cambios del remoto con la rama actual.
```

**Pull Requests (Solicitudes de Incorporación de Cambios):**
Una Pull Request (PR) es una herramienta fundamental para el desarrollo colaborativo. Permite proponer y revisar cambios antes de fusionarlos con la rama principal.

**Fork (Bifurcación):**
Un Fork es una copia de un repositorio existente en tu propia cuenta, ideal para contribuir a proyectos de código abierto.

### 2.2. Flujos de Trabajo

**GitFlow:**

- Es el flujo más completo y extendido.
- Define un esquema con ramas de larga duración como `main` (o `master`) y `develop`.
- Se crean ramas temporales para funcionalidades (*feature branches*), lanzamientos (*release branches*) y correcciones rápidas (*hotfix branches*).
- Es especialmente útil en proyectos que requieren ciclos de publicación organizados y un control claro sobre qué entra en cada versión.

> **Nota:**  
> Aunque existen otros flujos de trabajo, como **GitHub Flow** (más simple y orientado a despliegue continuo), en la mayoría de proyectos de aprendizaje y en entornos profesionales suele bastar con GitFlow.  
> Lo importante es que el equipo acuerde un único flujo de trabajo y lo siga de manera coherente.

### 2.3. Gestión de Proyectos con Scrum

En el desarrollo ágil, GitHub, Bitbucket y otras plataformas proporcionan herramientas que facilitan la aplicación de Scrum y la organización del trabajo en equipo:

1. **Issues (Incidencias: Tareas, Historias y Bugs)**  
   Se utilizan para registrar todo tipo de trabajo pendiente.

    - **Historias de usuario:** Describen funcionalidades desde la perspectiva del usuario final.
    - **Tareas:** Acciones concretas necesarias para implementar una historia.
    - **Bugs:** Errores o defectos detectados en el software.

   Los issues permiten asignar responsables, establecer prioridades, añadir etiquetas (labels) y relacionarlos con commits o pull requests.

2. **Tableros de Proyecto (Boards)**  
   Representan visualmente el flujo de trabajo del equipo mediante metodologías como Kanban o Scrum.

    - En un tablero típico se gestionan las columnas: Pendiente, En progreso, En revisión y Completado.
    - Facilitan la transparencia, la autoorganización y el seguimiento del avance de cada sprint.

3. **Wikis**  
   Sirven como repositorio de documentación complementaria. Se emplean para:

    - Manuales de uso.
    - Guías de instalación y despliegue.
    - Información técnica sobre la arquitectura del sistema.

   Su ventaja es que permanecen junto al repositorio y evolucionan con el proyecto.

4. **Confluence**  
   Herramienta colaborativa de Atlassian que se integra con Bitbucket y Jira. Permite crear documentación estructurada:

    - Actas de reuniones.
    - Backlogs detallados.
    - Diagramas y esquemas de arquitectura.

   Facilita la colaboración en tiempo real y mantiene centralizada la información del equipo.

### 2.4. Flujo de Trabajo Profesional: Del Commit a la Tarea (Issue)

Es fundamental la trazabilidad: cada commit debe estar vinculado a una tarea (issue).

Ejemplo de commit asociado a un issue:

```bash
git commit -m "feat: Añadido formulario de login (#15)"
```
Para cerrar automáticamente un issue al fusionar, usa palabras clave como:

- `closes #15`
- `fixes #15`
- `resolves #15`

### 2.5. Herramientas Complementarias

- **Visual Studio Code (VS Code):** Editor con integración Git.
- **GitKraken:** Interfaz gráfica avanzada para Git.
- **GitHub Desktop:** Cliente gráfico oficial de GitHub.

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