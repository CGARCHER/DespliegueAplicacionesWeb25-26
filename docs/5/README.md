# Despliegue de Sistema de Transferencia de Ficheros — UT5

Este tema pertenece al módulo profesional **Despliegue de Aplicaciones Web** del ciclo formativo de **2º de Desarrollo de Aplicaciones Web (DAW)**.  
Su finalidad es que el alumnado adquiera una comprensión detallada de los procesos, herramientas y protocolos necesarios para **mover archivos de forma segura entre cliente y servidor**, lo que les permitirá gestionar sistemas de transferencia de ficheros en entornos de producción.

## Contenido

- [5.1 Introducción y Hoja de Ruta](#51-introduccion-y-hoja-de-ruta)
- [5.2 Funcionamiento del Protocolo FTP](#52-funcionamiento-del-protocolo-ftp)
- [5.3 Modos de Conexión: Activo vs. Pasivo](#53-modos-de-conexion-activo-vs-pasivo)
- [5.4 Seguridad en FTP: El Enjaulamiento (Chroot)](#54-seguridad-en-ftp-el-enjaulamiento-chroot)
- [5.5 Comparativa entre FTPS y SFTP](#55-comparativa-entre-ftps-y-sftp)
- [5.6 Uso de Claves SSH para Autenticación Segura](#56-uso-de-claves-ssh-para-autenticacion-segura)
- [5.7 Aplicación Práctica en el Despliegue Web](#57-aplicacion-practica-en-el-despliegue-web)
- [5.8 Finalizando: Recomendaciones y Reflexiones](#58-finalizando)


## 5.1 Introducción y Hoja de Ruta

En esta unidad aprenderemos los fundamentos esenciales para mover archivos de forma segura por internet. Nuestro objetivo es comprender los métodos y protocolos clave utilizados en el despliegue de sistemas de transferencia de ficheros. Aprenderás sobre:
- Los cimientos del protocolo FTP y la evolución hacia FTPS y SFTP.
- El enfrentamiento con los cortafuegos y cómo utilizar los modos de conexión para evitarlos.
- El enjaulamiento de usuarios como medida de seguridad avanzada.
- La comparativa entre FTPS y SFTP y por qué SFTP es la mejor opción para entornos profesionales.

➡️ **Video complementario:** Puedes reforzar tu aprendizaje viendo este video: [Introducción al Despliegue de Sistemas de Transferencia de Ficheros](https://youtu.be/SNa24lCcMdc).

¡Prepárate para explorar los conceptos clave detrás de una transferencia de archivos segura, eficiente y moderna!


## 5.2 Funcionamiento del FTP (El protocolo veterano)

El **FTP (Protocolo de Transferencia de Archivos)** es uno de los métodos más antiguos para mover archivos entre un cliente y un servidor. Aunque es eficiente, fue diseñado sin tener en mente la seguridad, lo que lo hace vulnerable a ataques.

FTP utiliza *dos tipos de conexiones distintas*:
- **Canal de control (Puerto 21):** Maneja las órdenes e inicios de sesión del cliente al servidor.
- **Canal de datos:** Transfiere los archivos reales. Este canal suele abrirse en un rango de puertos dinámicos.

**Nota importante:** Debido a que FTP transmite datos y credenciales en texto plano, ¡nunca debe usarse sin capas adicionales de seguridad!

## 5.3 Modos de Conexión: Activo vs. Pasivo

Para comprender cómo funciona FTP con los cortafuegos, utilizaremos la analogía de un almacén:

### Modo Activo
- El cliente pide el archivo al servidor utilizando el puerto 21.
- El servidor entonces intenta conectarse al cliente en un puerto aleatorio para transferir el archivo.
- **Problema:** Si el cortafuegos del cliente no reconoce la conexión entrante, la conexión falla.

### Modo Pasivo
- El servidor informa al cliente sobre un puerto específico (el "muelle de carga") donde puede recoger el archivo.
- El cliente inicia la conexión al puerto indicado.
- **Ventaja:** La conexión suele funcionar bien incluso con cortafuegos activos.
- **Desventaja:** El servidor necesita abrir un rango amplio de puertos, incrementando su superficie de ataque.

**Consejo práctico:** Utiliza el modo pasivo siempre que trabajes con clientes de FTP modernos para evitar problemas con cortafuegos.


## 5.4 Seguridad: El Enjaulamiento (Chroot)

El **enjaulamiento de usuarios** es una técnica que limita a cada usuario a su directorio personal. Esto evita que puedan explorar (o comprometer) otras partes del servidor.

En servidores FTP como **vsftpd**, se activa agregando la siguiente línea al archivo de configuración:
```
chroot_local_user=YES
```

### ¿Por qué es importante?
- Impide que un usuario malintencionado acceda a archivos de otros usuarios o al sistema completo.
- Refuerza la seguridad del servidor, especialmente en entornos multiusuario.


## 5.5 La Gran Batalla: FTPS vs. SFTP

Es común confundir estos dos protocolos, pero sus diferencias son clave:

### FTPS
- **Qué es:** FTP con una capa de seguridad SSL/TLS.
- **Ventajas:**
    - Añade cifrado al FTP tradicional.
- **Desventajas:**
    - Utiliza múltiples puertos para las conexiones, lo que lo hace menos práctico con cortafuegos.
    - Más complejo de configurar correctamente.

### SFTP
- **Qué es:** Un protocolo completamente diferente basado en SSH.
- **Ventajas:**
    - Utiliza un único puerto (22) para todas las conexiones.
    - Todas las transferencias y credenciales están cifradas por defecto.
    - Más fácil de configurar y mantener.
- **Desventajas:**
    - Ninguna significativa para despliegues modernos.

**Conclusión:** SFTP es la elección profesional para despliegues web gracias a su simplicidad, seguridad y soporte universal.

## 5.6 Autenticación mediante Claves SSH

Una de las fortalezas de **SFTP** es su capacidad para autenticar usuarios mediante claves SSH, eliminando los riesgos asociados a las contraseñas tradicionales.

### ¿Cómo funciona?
- **Clave Pública:** Actúa como la *cerradura*. Se almacena en el servidor.
- **Clave Privada:** Es la *llave única* que guarda el usuario de forma segura.

**Ventajas de usar claves SSH:**
- Previenen ataques por fuerza bruta.
- Eliminan la necesidad de recordar contraseñas.
- Ofrecen un nivel de seguridad significativamente mayor.

**Consejo:** Usa herramientas como `ssh-keygen` para generar tus claves SSH y configúralas en tus servidores. Siempre protege tu clave privada con una contraseña segura.


## 5.7 Aplicación en el Despliegue Web

En el mundo real del desarrollo, estos conceptos son fundamentales para subir tus aplicaciones al servidor de producción:

- Por ejemplo, al desplegar una aplicación de **SpringBoot**, deberías usar **SFTP** para transferir los archivos al servidor de producción.
- Configura las claves SSH para conectar de forma segura tu máquina de desarrollo con el servidor.

### ¿Por qué es importante?
- **Fiabilidad:** SFTP garantiza que tus archivos lleguen completos y sin modificaciones.
- **Seguridad:** Las conexiones están protegidas contra amenazas comunes.


## 5.8 Finalizando

La transferencia segura de archivos es un componente esencial del despliegue de aplicaciones web. Adoptar prácticas seguras, como el uso de SFTP y claves SSH, no solo protege tu aplicación, sino que también demuestra profesionalismo y compromiso con la calidad.
No arriesgues la seguridad de tu servidor con contraseñas simples cuando puedes usar herramientas robustas como SSH. Cada pequeño esfuerzo para mejorar la seguridad cuenta y protege tanto tus datos como tu reputación profesional.

### Algunos briconsejos adicionales:
1. **Utiliza logs para monitorear las transferencias:** Te ayudarán a verificar conexiones activas en tu servidor.
2. **Mantén el servidor actualizado:** Esto incluye tanto el sistema operativo como el software de tu servidor FTP/SFTP.
3. **Desactiva accesos innecesarios:** Si no estás utilizando FTP o FTPS, desactívalos para reducir vectores de ataque.# Despliegue de Sistema de Transferencia de Ficheros — UT5

Este tema pertenece al módulo profesional **Despliegue de Aplicaciones Web** del ciclo formativo de **2º de Desarrollo de Aplicaciones Web (DAW)**.  
Su finalidad es que el alumnado adquiera una comprensión detallada de los procesos, herramientas y protocolos necesarios para **mover archivos de forma segura entre cliente y servidor**, lo que les permitirá gestionar sistemas de transferencia de ficheros en entornos de producción.