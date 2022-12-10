---
layout: default
---

TP de Taller de Programación 2, 2do. cuatrimestre de 2022 - FIUBA.

# Introducción

En este trabajo práctico se desarrolló **FIUBER**, una plataforma que conecta pasajeros con choferes en tiempo real,
la cual consiste en una aplicación mobile, un backoffice web para administradores y un conjunto de servicios backend.
Los repositorios de código pueden ser encontrados en la [GitHub Organization](https://github.com/TallerDeProgramacion2-2022-2c-Grupo7) del proyecto.

* * *

# Arquitectura

La aplicación mobile fue desarrollada en [React Native](https://reactnative.dev/) para el sistema operativo **Android**, y para el backoffice
se utilizó [React](https://es.reactjs.org/). Ambas aplicaciones hacen uso de [Firebase](https://firebase.google.com/?hl=es) para el registro
y autenticación de usuarios.

Para la construcción del backend de la plataforma, se diseñó una arquitectura basada en microservicios como se puede
observar en el siguiente diagrama:

![image](https://user-images.githubusercontent.com/43656633/206757211-2110b8c6-ba94-4de5-901e-32de7e932ef4.png)

Cada microservicio expone sus funcionalidades mediante una API, la cual implementa autorización por token mediante
la librería de [Firebase Admin](https://firebase.google.com/docs/admin/setup).
Estos microservicios fueron implementados siguiendo el principio de responsabilidad única y son los siguientes:

- **Trips**: Creación, actualización y cotización de viajes.
- **Ratings**: Calificación de pasajeros y choferes.
- **Metrics**: Generación de eventos para métricas.
- **Notification PIN**: Envío de códigos de verificación de usuarios.
- **Wallet**: Pago y cobro de viajes.
- **Users**: Gestión de usuarios para administradores.

Cada servicio contiene en su repositorio las instrucciones de instalación y uso. Además cuentan con un archivo `Dockerfile` que
permite ejecutarlos sin necesidad de tener las dependencias instaladas.

* * *

# CI-CD

Cada repositorio cuenta con uno o más workflows de integración continua usando la plataforma de [GitHub Actions](https://docs.github.com/en/actions).

En primera instancia, cuando se abre un pull request a la rama de `main`, se corren todas las pruebas para asegurar
la integridad del código y evitar errores en el ambiente productivo.

Una vez que las pruebas ejecutaron correctamente y no se reportaron errores, el siguiente paso es realizar
el merge a `main`. En este paso se ejecuta el despliegue, en el caso de la aplicación mobile, este
consiste en la generación del archivo `.apk`.

![image](https://user-images.githubusercontent.com/43656633/206767428-b285817a-ac5b-46e0-93de-bc06e664a194.png)

En el caso de los servicios backend, el despliegue se realiza en kubernetes, para el cual se eligió la plataforma
Okteto. Para esto, cada repositorio cuenta con un archivo `kubernetes.yml` que define los recursos a
crear en el clúster, los cuales son un servidor web, y en los casos que corresponda, un servidor de base de datos
y un volumen de datos. Este último recurso asegura la persistencia de los datos a través de todos los despliegues.

![image](https://user-images.githubusercontent.com/43656633/206767003-0e21fb5f-6b74-4894-a1df-bb8559d0b068.png)

* * *

# Monitoreo

Para el monitoreo de eventos y visualización de métricas de uso de los servicios desplegados en la nube,
se utilizó la plataforma [Datadog](https://www.datadoghq.com/). Esta plataforma ofrece un servicio de generación de eventos mediante
el uso de un *Agent* que corre en el clúster de Okteto. Cada servicio se encarga, mediante la librería correspondiente,
de enviar los mensajes relacionados a cada request, ya sea a modo informativo o para reportar un error. Luego
estos mensajes se pueden visualizar como en la siguiente imagen:

![image](https://user-images.githubusercontent.com/43656633/206869627-7b3e16eb-6caa-413c-8c5b-8445115c8f0a.png)

Esta plataforma permite además el monitoreo de la infraestructura, generando métricas de uso de recursos:

![image](https://user-images.githubusercontent.com/43656633/206871527-e2277cba-2de9-4dad-ab24-016ef6b2572c.png)

* * *

# Bitácora del proyecto

El detalle de las actividades realizadas para cada entrega se puede encontrar en la siguiente [página](https://github.com/orgs/TallerDeProgramacion2-2022-2c-Grupo7/projects/1/views/8).

* * *

# Análisis postmortem

* * *
