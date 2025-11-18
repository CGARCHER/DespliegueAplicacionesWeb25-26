Práctica: Configuración de un dominio local y despliegue mediante Apache + Tomcat (HTTP y HTTPS)

Esta práctica consiste en configurar un dominio local (por ejemplo, midominio.local) y utilizar Apache como servidor web frontal actuando como reverse proxy, reenviando las peticiones hacia una aplicación desplegada en Tomcat. También incluye la configuración opcional de HTTPS utilizando un certificado autofirmado. La práctica puede realizarse tanto con una instalación independiente de Apache como con el servidor Apache incluido en XAMPP.
1. Requisitos previos

- Tomcat funcionando
- Una aplicación accesible en:
  http://localhost:8080/despliegue-api-rest-0.0.1-SNAPSHOT/
- Esto se hace en la parte anterior.
- Apache instalado:
    - Apache Lounge (C:\Apache24\)
    - o XAMPP (C:\xampp\apache\)

2. Crear un dominio local

Editar el archivo:

```
C:\Windows\System32\drivers\etc\hosts
```

Añadir la línea:

```
127.0.0.1   pets.local
```

Con esto, cualquier acceso al dominio pets.local se dirigirá al propio equipo.

3. Activar VirtualHosts y módulos de proxy en Apache

En el archivo:

```
C:\Apache24\conf\httpd.conf
```

o

```
C:\xampp\apache\conf\httpd.conf
```

Asegurarse de activar:

```
Include conf/extra/httpd-vhosts.conf
```

Y los módulos necesarios para usar Apache como reverse proxy:

```
LoadModule proxy_module modules/mod_proxy.so
LoadModule proxy_http_module modules/mod_proxy_http.so
```

4. Configurar el VirtualHost HTTP

Editar:

```
C:\Apache24\conf\extra\httpd-vhosts.conf
```

o

```
C:\xampp\apache\conf\extra\httpd-vhosts.conf
```

Añadir:

```
<VirtualHost *:80>
    ServerName pets.local

    ErrorLog "logs/pets-error.log"
    CustomLog "logs/pets-access.log" common

    ProxyPreserveHost On
    ProxyPass        "/" "http://localhost:8080/despliegue-api-rest-0.0.1-SNAPSHOT/"
    ProxyPassReverse "/" "http://localhost:8080/despliegue-api-rest-0.0.1-SNAPSHOT/"
</VirtualHost>
```

Explicación breve de los logs

- ErrorLog: guarda mensajes de error del servidor (fallos del proxy, rutas mal configuradas, módulos incorrectos, etc.).
  Es el archivo que se consulta cuando “algo no funciona”.

- CustomLog / access.log: guarda todas las peticiones que llegan al sitio: rutas, códigos 200/404/500, navegador, fecha…
  Sirve para comprobar si las peticiones llegan realmente a Apache y qué devuelve.

Estos archivos permiten ver qué ocurre en el servidor de forma clara.

5. Comprobar configuración y reiniciar Apache

Comprobar que no hay errores:

```
httpd.exe -t
```

Reiniciar:

```
httpd.exe -k restart
```

En XAMPP basta con reiniciar Apache desde el panel.

6. Probar el dominio

Abrir:

```
http://pets.local
```

La aplicación debería aparecer usando el dominio local, aunque realmente está siendo servida por Tomcat a través de Apache.

Hasta aquí llega la parte obligatoria de la práctica.

7. Extensión opcional: Activar HTTPS con un certificado autofirmado

7.1 Qué es un certificado autofirmado

Un certificado autofirmado es un certificado generado por el propio servidor, sin una entidad certificadora externa.
La conexión sigue estando cifrada, pero el navegador mostrará un aviso indicando que el certificado no es de confianza.
Es lo normal en prácticas locales y no supone riesgo en este contexto.

7.2 Activar SSL en Apache

En httpd.conf activar:

```
LoadModule ssl_module modules/mod_ssl.so
LoadModule socache_shmcb_module modules/mod_socache_shmcb.so

Listen 443
```

7.3 Crear los certificados

El ejecutable necesario está en:

- Apache: `C:\Apache24\bin\openssl.exe`
- XAMPP: `C:\xampp\apache\bin\openssl.exe`

Crear una carpeta para guardarlos:

```
C:\Apache24\conf\certs\
```

Generar la clave privada:

```
openssl.exe genrsa -out C:\Apache24\conf\certs\server.key 2048
```

Generar el certificado:

```
openssl.exe req -new -x509 -key C:\Apache24\conf\certs\server.key -out C:\Apache24\conf\certs\server.crt -days 365
```

Ejemplo de datos:

- Country: ES
- State: Madrid
- Locality: Madrid
- Organization: Departamento DAW
- Unit: Despliegue Web
- Common Name: pets.local
- Email: admin@pets.local

El valor más importante es Common Name, que debe coincidir con el dominio.

7.4 Crear el VirtualHost HTTPS

Añadir:

```
<VirtualHost *:443>
    ServerName pets.local

    SSLEngine on
    SSLCertificateFile "C:/Apache24/conf/certs/server.crt"
    SSLCertificateKeyFile "C:/Apache24/conf/certs/server.key"

    ErrorLog "logs/pets-ssl-error.log"
    CustomLog "logs/pets-ssl-access.log" common

    ProxyPreserveHost On
    ProxyPass        "/" "http://localhost:8080/despliegue-api-rest-0.0.1-SNAPSHOT/"
    ProxyPassReverse "/" "http://localhost:8080/despliegue-api-rest-0.0.1-SNAPSHOT/"
</VirtualHost>
```

7.5 Probar en el navegador

Acceder a:

```
https://pets.local
```
