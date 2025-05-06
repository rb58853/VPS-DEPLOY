# Docker

Docker es una herramienta que permite empaquetar y ejecutar aplicaciones en contenedores docs.docker.com. Es como tener una caja pequeña y ligera donde puedes meter toda tu aplicación junto con todo lo que necesita para funcionar.

### ¿Qué son los contenedores?

- Son entornos independientes y aislados
- Contienen la aplicación y todas sus dependencias
- Son más ligeros que las máquinas virtuales tradicionales
- Comparten el mismo sistema operativo base

Esta opción será considerada como una alternativa idónea para el almacenamiento de proyectos. Docker no mantiene una relación directa con servicios en la nube ni servidores, sino que constituye una metodología especializada para organizar tanto los entornos como las aplicaciones, con el propósito posterior de implementarlas en los servidores correspondientes.

## Conceptos y Herramientas Importantes

Es fundamental conocer cada uno de los elementos fundamentales que se utilizarán en este procedimiento.

- **[Dockerfile](#dockerfile):** Documento de configuración que define las instrucciones específicas para crear una imagen de Docker, permitiendo la creación sistemática y reproducible de entornos de desarrollo y producción.
- **[Docker Compose](#docker-compose):** Herramienta de orquestación que facilita la definición y ejecución de múltiples contenedores Docker, simplificando la gestión de aplicaciones complejas mediante la configuración declarativa de servicios interconectados.
- **[GitHub y GitHub Actions](#github):** Plataforma de control de versiones que, combinada con GitHub Actions, permite automatizar flujos de trabajo completos, desde pruebas hasta despliegues, integrándose perfectamente con el ecosistema Docker para implementar pipelines CI/CD robustos.

## DockerFile

Un Dockerfile es como una receta paso a paso que le dice a Docker cómo construir una imagen de contenedor. Es un archivo de texto que contiene instrucciones simples que Docker sigue secuencialmente.

### Estructura básica

    FROM ubuntu:latest
    COPY . /app
    WORKDIR /app
    RUN apt-get update && apt-get install -y python3
    CMD ["python3", "app.py"]

#### Explicación de comandos comunes

- `FROM`: Indica la imagen base que vamos a usar
- `COPY`: Copia archivos desde nuestro computador al contenedor
- `WORKDIR`: Establece el directorio de trabajo dentro del contenedor
- `RUN`: Ejecuta comandos durante la construcción de la imagen
- `CMD`: Define el comando predeterminado cuando se ejecuta el contenedor

### Casos de uso

En nuestro caso usaremos dockerFile para crear nuestra imagen de docker, imagen que guardaremos en dockerhub y luego cargaremos desde aqui usando un docker compose en nuestro VPS, de esta forma levantaremos la imagen creada en nuestro VPS. Este proceso se automatiza siguiendo [estos pasos](./VPS.md#automatizacion-en-github).

## Docker Compose

TODO: Explicar todo esto en talla
Es necesario instalar docker compose para poder usar los `.ylm`.

```shell
sudo apt-get install docker-compose-plugin -y
```

Luego levantar el contenedor con el bash:

```shell
docker compose up
```
