# Práctica: Instalación y despliegue con Apache Tomcat (Windows)

Última actualización: 2025-11-12  
Autor: CGARCHER

Descripción
----------
Este documento explica cómo descargar, instalar y configurar Apache Tomcat en Windows, cómo habilitar el Manager App, aumentar el límite de subida de WARs, desplegar aplicaciones (incluyendo un proyecto Spring Boot empaquetado como WAR) y cómo funciona el autodeploy (despliegue automático al copiar el WAR en la carpeta webapps).

Instalación (Windows)
----------------------------

1) Descargar y descomprimir Tomcat
---------------------------------
1. Ve a: https://tomcat.apache.org/download-10.cgi
2. Descarga la versión "Core" (archivo zip) para Tomcat 10.x.
3. Descomprime el zip en una ruta sin espacios recomendada, por ejemplo:
    - `C:\tomcat`  
      Evita rutas con caracteres especiales.

2) Configurar variables de entorno (Windows)
-------------------------------------------
Abre PowerShell como Administrador o usa CMD para ejecutar comandos `setx`.

Ejemplo (desde PowerShell o CMD):
```bat
setx JAVA_HOME "C:\Program Files\Java\jdk-17"
```
o define la variable en entorno de windows.

3) Iniciar y detener Tomcat (Windows)
-------------------------------------
Desde la carpeta `C:\tomcat\bin`:

Iniciar:
```bat
startup.bat
```

Detener:
```bat
shutdown.bat
```

Abrir en el navegador:
http://localhost:8080

Logs principales:
- `C:\tomcat\logs\catalina.out`
- `C:\tomcat\logs\catalina.<date>.log`
- `C:\tomcat\logs\localhost.<date>.log`

4)Configurar acceso al Manager App (usuario y roles)
-----------------------------------------------------
Edita el archivo:
`C:\tomcat\conf\tomcat-users.xml`

Añade (dentro de `<tomcat-users>`):
```xml
<role rolename="manager-gui"/>
<role rolename="admin-gui"/>
<user username="admin" password="admin123" roles="manager-gui,admin-gui"/>
```
- Reinicia Tomcat (`shutdown.bat` y `startup.bat`).
- Accede a: http://localhost:8080/manager/html  
  Inicia sesión con:
    - Usuario: `admin`
    - Contraseña: `admin123`

Seguridad: no uses credenciales por defecto en producción. Usa contraseñas fuertes y restringe acceso por IP si es posible.

5) Permitir subir WARs > 50 MB (Windows)
----------------------------------------
Por defecto, el Manager limita subidas a 50MB. Para aumentarlo:

A) Modificar `web.xml` del Manager:
`C:\tomcat\webapps\manager\WEB-INF\web.xml`

Busca o añade el bloque `<multipart-config>` y ajusta:
```xml
<multipart-config>
  <max-file-size>209715200</max-file-size>        <!-- 200 MB -->
  <max-request-size>209715200</max-request-size>  <!-- 200 MB -->
  <file-size-threshold>0</file-size-threshold>
</multipart-config>
```

Reinicia Tomcat después de cualquier cambio en `conf`.

6)Desplegar una aplicación (.war) en Windows
---------------------------------------------
Opción A — Manager Web:
1. Abre: http://localhost:8080/manager/html
2. En la sección "Deploy" → "WAR file to deploy" → "Choose file" → Subir y Deploy.

Opción B — Copiar a la carpeta webapps (Autodeploy):
1. Copia `mi-aplicacion.war` a:
   ```
   C:\tomcat\webapps\
   ```
2. Si Tomcat tiene autodeploy activado, detectará el nuevo WAR, lo descomprimirá y desplegará automáticamente.
3. Accede a: http://localhost:8080/mi-aplicacion

Nota: Si nombras el archivo `ROOT.war`, la aplicación se desplegará en la raíz (`http://localhost:8080/`).

Autodeploy (despliegue automático)
---------------------------------
Tomcat puede desplegar automáticamente aplicaciones cuando detecta cambios en la carpeta `webapps`. En Windows esto permite desplegar simplemente copiando el archivo WAR a `C:\tomcat\webapps\`.

Configuración de Host (por defecto suele estar activado)
`C:\tomcat\conf\server.xml` contiene una sección `<Host ...>` similar a:
```xml
<Host name="localhost" appBase="webapps"
      unpackWARs="true" autoDeploy="true" deployOnStartup="true">
    ...
</Host>
```
- unpackWARs="true": Tomcat descomprime el WAR en una carpeta con el mismo nombre.
- autoDeploy="true": Tomcat vigila cambios en `appBase` en caliente y despliega nuevas aplicaciones.
- deployOnStartup="true": Tomcat despliega las aplicaciones que encuentra al arrancar.

Cómo funciona en la práctica (Windows)
- Copia `mi-aplicacion.war` a `C:\tomcat\webapps\`. Tomcat detecta el WAR y crea `C:\tomcat\webapps\mi-aplicacion\` con los ficheros descomprimidos.
- Para actualizar una aplicación, puedes:
    - Parar Tomcat, reemplazar el WAR y arrancar (más seguro).
    - O bien: eliminar la carpeta descomprimida `mi-aplicacion` y copiar el nuevo WAR (Tomcat detectará el nuevo WAR y lo desplegará).
    - O usar Manager App / Manager Text API para hacer deploy/update remoto.
- Si eliminas el WAR, Tomcat puede auto-undeploy si `autoDeploy` está activado (también eliminará la carpeta desplegada). Si solo borras la carpeta descomprimida, Tomcat no restaurará el WAR.

7)Preparar un proyecto Spring Boot para Tomcat externo (Windows)
-----------------------------------------------------------------
Por defecto Spring Boot usa Tomcat embebido. Para desplegar en Tomcat externo:

- pom.xml: cambiar packaging y gestionar la dependencia de Tomcat
```xml
<packaging>war</packaging>
```

- Clase principal: extender SpringBootServletInitializer
```java
@SpringBootApplication
public class MiAplicacion extends SpringBootServletInitializer {

    @Override
    protected SpringApplicationBuilder configure(SpringApplicationBuilder application) {
        return application.sources(MiAplicacion.class);
    }

    public static void main(String[] args) {
        SpringApplication.run(MiAplicacion.class, args);
    }
}
```
- Empaquetar con Maven (PowerShell/CMD, en el directorio del proyecto):
```bat
mvn package o mvn install
```
- Copia `target\mi-aplicacion.war` a `C:\tomcat\webapps\` o súbelo via Manager.

Recursos
--------
- Apache Tomcat 10: https://tomcat.apache.org/download-10.cgi
- Manager App howto: https://tomcat.apache.org/tomcat-10.0-doc/manager-howto.html
- Spring Boot WAR deployment: https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#howto-traditional-deployment-war
