# Manual de Uso Básico de Jira con Ejemplo Práctico para Equipos de Desarrollo

## ¿Qué es Jira?

[Jira](https://www.atlassian.com/software/jira) es una herramienta de gestión de proyectos desarrollada por Atlassian, ampliamente utilizada por equipos de desarrollo de software para organizar, priorizar y realizar el seguimiento de tareas, bugs, requisitos y proyectos completos. Jira facilita la colaboración, la transparencia y el control del avance en equipos ágiles (Scrum, Kanban), permitiendo visualizar el trabajo pendiente, en progreso y completado.

---

## ¿Para qué sirve Jira en un equipo de desarrollo?

- **Planificación:** Permite crear proyectos, definir epics, requisitos y tareas.
- **Seguimiento:** Visualiza el estado de cada tarea en tableros (por ejemplo, Scrum o Kanban).
- **Colaboración:** Asigna responsables, agrega comentarios, adjunta archivos y notifica cambios.
- **Transparencia:** Monitorea avances, identifica bloqueos y facilita la entrega continua.

---

## Ejemplo Práctico: Desarrollo de una Funcionalidad de Login

### 1. Crear un Proyecto

1. Ingresa a Jira y selecciona “Crear proyecto”.
2. Elige la plantilla (por ejemplo, “Scrum”).
3. Nombra el proyecto (ej: “Desarrollo App Web”).
4. Configura y crea el proyecto.

---

### 2. Crear un Requisito (Ejemplo: Usuario debe poder iniciar sesión)

**Paso a paso:**

1. Ingresa a tu proyecto en Jira.
2. Haz clic en el botón “Crear” o “Create” (generalmente arriba a la izquierda).
3. En “Tipo de issue” selecciona “Requisito” o similar (puede aparecer como “Historia”, “Story” o “User Story” según la configuración de tu Jira).
4. Completa los campos:
    - **Resumen:** Como usuario, quiero poder iniciar sesión en la aplicación utilizando mi correo electrónico y contraseña, para acceder a mis datos personales de forma segura.
    - **Descripción:** Detalla el requisito y agrega los criterios de aceptación:
        1. Debe existir un formulario de login con campos para email y contraseña.
        2. El sistema debe validar que el email esté registrado y la contraseña sea correcta.
        3. Si los datos son correctos, el usuario debe acceder a su área personal.
        4. Si los datos no son correctos, se debe mostrar un mensaje de error.
        5. La comunicación entre el frontend y backend debe ser segura.
    - **Asignar:** (Opcional) Asigna a la persona responsable.
    - **Etiquetas:** (Opcional) Añade etiquetas como `frontend`, `backend`, etc.
    - **Puntos de historia:** (Opcional, si está habilitado) Estima los puntos de historia.
5. Haz clic en “Crear”.

---

### 3. Desglose en Tareas por Equipo

**Paso a paso para crear tareas o subtareas:**

1. Dentro del requisito recién creado, abre el detalle del ticket.
2. Busca la opción “Crear sub-tarea” (Create sub-task) o “Vincular issue” (Link issue) para tareas separadas.
3. Para cada tarea:
    - En “Tipo de issue” selecciona “Tarea” o “Sub-tarea” según corresponda.
    - **Resumen:** Describe brevemente la tarea (Ej: Crear endpoint de autenticación).
    - **Descripción:** Explica lo necesario para completarla.
    - **Asignar:** (Opcional) Elige al responsable.
    - **Puntos de historia:** (Opcional) Estima el esfuerzo.
4. Repite el proceso para cada una de las tareas identificadas en el desglose.
5. Asígnalas y verifica que estén vinculadas al requisito principal.

#### Backend (ejemplo de tareas):
- Crear endpoint de autenticación (`POST /api/login`) que reciba email y contraseña.
- Validar las credenciales contra la base de datos.
- Generar y devolver un token JWT si el login es exitoso.
- Manejar casos de error (usuario no existe, contraseña incorrecta).
- Documentar el endpoint.

#### Frontend (ejemplo de tareas):
- Crear el formulario de login (campos email y contraseña, botón de enviar).
- Validar los campos (formato de email, campos obligatorios).
- Consumir el endpoint de autenticación del backend.
- Guardar el token JWT en almacenamiento seguro.
- Mostrar mensajes de error apropiados según la respuesta del backend.
- Redirigir al usuario al dashboard si el login es correcto.

---

### 4. Estimación de Tareas con Puntos de Historia

En Jira, es recomendable estimar cada requisito y tarea con **puntos de historia**, que representan el esfuerzo relativo para completarlos. Los puntos de historia ayudan a:

- Medir la capacidad del equipo en un sprint.
- Priorizar tareas importantes.
- Mejorar la planificación y previsibilidad.

**¿Cómo hacerlo en Jira?**

1. Al crear el requisito o la tarea, busca el campo “Puntos de historia” y asígnale un valor según consenso del equipo (ejemplo: 1, 2, 3...).
2. Si no ves el campo, pídele al administrador que lo active para tu proyecto.

**Ejemplo de estimaciones:**

| Tarea                                          | Estimación (Puntos de historia) |
|------------------------------------------------|-------------------------------|
| Crear endpoint de autenticación (backend)      | 3                             |
| Validar credenciales y JWT (backend)           | 2                             |
| Documentar endpoint (backend)                  | 1                             |
| Crear formulario de login (frontend)           | 2                             |
| Validar campos y consumir API (frontend)       | 3                             |
| Manejar errores y redirección (frontend)       | 2                             |

**¿Qué es importante?**
- Las tareas con mayor puntuación (por ejemplo, 3) suelen ser más complejas o críticas.
- Prioriza tareas que desbloquean otras, por ejemplo, el endpoint de autenticación debe estar listo antes de que el frontend pueda integrarlo.

---

### 5. Crear y Arrancar un Sprint en Jira

**Paso a paso:**

1. Ve a tu proyecto y selecciona “Backlog”.
2. Arrastra los requisitos y tareas estimadas (con sus subtareas) al “Sprint 1”.
3. Define la duración del sprint (ejemplo: 2 semanas).
4. Verifica la suma de puntos de historia asignados al sprint y compárala con la capacidad de tu equipo (por ejemplo, si tu equipo puede completar 10 puntos por sprint, no agregues más de 10 puntos).
5. Haz clic en “Iniciar Sprint”.

---

### 6. Seguimiento del Sprint y del Tablero

- Usa el tablero (“Board”) de Jira para mover las tareas entre los estados: “Por hacer” (To Do), “En progreso” (In Progress), “Hecho” (Done).
- Cada integrante arrastra sus tareas conforme avanza.
- Revisa el “Burndown Chart” para visualizar el progreso del equipo y si el sprint avanza según lo planeado.
- Realiza reuniones diarias (dailies) para identificar bloqueos y coordinar acciones.

---

### 7. Consejos y Recursos

- Usa etiquetas para organizar tareas (por ejemplo: `frontend`, `backend`, `alta prioridad`).
- Prioriza requisitos y tareas según el valor para el usuario.
- Realiza revisiones periódicas del tablero para gestionar bloqueos y dependencias.
- Aprovecha la documentación y recursos oficiales:
    - [Guía oficial de Jira (Atlassian)](https://support.atlassian.com/jira-software-cloud/docs/)
    - [Curso básico de Jira en español (YouTube)](https://www.youtube.com/results?search_query=jira+curso+español)

---

**Este manual es una guía básica y puede adaptarse a las necesidades y procesos de tu equipo de desarrollo.**