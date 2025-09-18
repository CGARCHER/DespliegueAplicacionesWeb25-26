# Despliegue de Aplicaciones Web - UT1
## Control de Versiones y Documentación

---

En el cambiante panorama del desarrollo de software, existen dos fundamentos que resultan imprescindibles para garantizar proyectos eficientes, colaborativos y de calidad: el **control de versiones** y la **documentación del código**.  
A lo largo de este tema analizaremos cómo herramientas como **Git** y **GitHub** han transformado la manera en que los equipos organizan y supervisan sus proyectos, así como la importancia de una documentación clara y estandarizada para asegurar la mantenibilidad y el éxito a largo plazo de cualquier aplicación.

---

## 📚 Contenido

- **Git: El Sistema de Control de Versiones**
    - 1.1. Introducción a Git
    - 1.2. Conceptos Clave en Git
    - 1.3. Ciclo de Vida de los Archivos en Git
    - 1.4. Comandos Git Esenciales con Ejemplos Prácticos
- **Plataformas de Colaboración: GitHub y Bitbucket**
    - 2.1. Colaboración en la Nube
    - 2.2. Flujos de Trabajo Comunes
    - 2.3. Gestión de Proyectos con Scrum
    - 2.4. Flujo de Trabajo Profesional
    - 2.5. Herramientas Complementarias
- **Documentación de Código**
    - 3.1. Markdown
    - 3.2. JavaDoc (Java)

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
2. **Preparar archivos:** Añades los archivos modificados al área de preparación. Esto le indica a Git qué cambios específicos deseas incluir en la próxima instantánea.
3. **Confirmar cambios:** Realizas un commit, lo que toma los archivos tal como están en el área de preparación y almacena esa instantánea de forma permanente en tu repositorio local.

**Estados principales:**
- **Confirmado (Committed):** Los datos están almacenados de forma segura en tu base de datos local.
- **Modificado (Modified):** Has modificado un archivo en tu directorio de trabajo, pero aún no lo has añadido al área de preparación.
- **Preparado (Staged):** Has marcado un archivo modificado para que se incluya en la próxima confirmación.

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

### 2.2. Flujos de Trabajo Comunes

- **GitHub Flow:** Ramas cortas, revisión por Pull Request, despliegue continuo.
- **GitFlow:** Ramas de larga vida (`main`/`master`, `develop`), ramas temporales para funcionalidades, lanzamientos y correcciones rápidas.

### 2.3. Gestión de Proyectos con Scrum

- **Issues (Tareas, Historias y Bugs):** Se gestionan como incidencias (issues) en GitHub/Bitbucket.
- **Tableros de Proyecto (Boards):** Visualización del estado del trabajo (kanban, scrum).
- **Wikis:** Documentación adicional del proyecto.
- **Confluence:** Espacio colaborativo para documentación estructurada, integrado con Bitbucket/Jira.

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

La documentación del código es esencial para el mantenimiento futuro y la colaboración en cualquier proyecto.

### 3.1. Markdown

Markdown es un lenguaje de marcado ligero para dar formato a texto plano.  
Es el formato estándar para archivos README.md y wikis.

**Ventajas:**
- Facilidad de lectura y escritura.
- Versatilidad y portabilidad.
- Soporte y popularidad en la comunidad.

**Sintaxis Básica:**

```markdown
# Título Principal (H1)
## Sección Importante (H2)

**Negrita** y *Cursiva*

- Lista con viñetas
1. Lista numerada

[Enlace a Google](https://www.google.com)

![Logo de Ejemplo](/assets/logo.png)

`código en línea`

```java
// Bloque de código Java
public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hola, Mundo!");
    }
}
```

| Encabezado 1 | Encabezado 2 |
|--------------|--------------|
| Fila 1 Col 1 | Fila 1 Col 2 |
| Fila 2 Col 1 | Fila 2 Col 2 |
```

### 3.2. JavaDoc (Java)

JavaDoc es la utilidad estándar de Java para generar documentación de APIs en formato HTML directamente del código fuente.  
Los comentarios JavaDoc se escriben con la sintaxis especial `/** ... */` y etiquetas `@`.

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
     * @return El {@code String} que representa el ID del producto.
     */
    public String getId() {
        return id;
    }

    /**
     * Establece el precio del producto.
     * @param precio El nuevo precio del producto (debe ser mayor que cero).
     * @throws IllegalArgumentException Si el precio proporcionado es menor o igual a cero.
     */
    public void setPrecio(double precio) {
        if (precio <= 0) {
            throw new IllegalArgumentException("El precio debe ser mayor que cero.");
        }
    }
}
```

**Para generar la documentación:**

```bash
javadoc -d docs src/main/java/com/mi-proyecto/*.java
```

---

¡Listo! Puedes copiar y pegar este documento en el archivo `README.md` de tu repositorio.