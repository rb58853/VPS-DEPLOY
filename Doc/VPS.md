# VPS (Servidor Privado Virtual)

Un VPS es un entorno virtual aislado que funciona como un servidor dedicado dentro de un servidor físico compartido
. Es como tener una habitación privada en un restaurante, donde tienes control total sobre tu espacio aunque compartas el edificio con otros
.

### Características Principales

- Recursos dedicados garantizados
- Mayor control y personalización
- Mejor rendimiento que el hosting compartido
- Capacidad de escalabilidad
- Mayor seguridad y aislamiento

### Sistema Operativo

Se suele instalar un sistema operativo de preferencia (Linux o Windows)
, permitiendo:

- Control total sobre el entorno
- Instalación de software específico
- Configuración personalizada
- Gestión de recursos asignados

En este caso particular se estara trabajando sobre un VPS con el sistema operativo instalado: [Ubuntu 24.04](https://ubuntu.com/blog/tag/ubuntu-24-04-lts)

# Instalacion de bibliotecas y herramientas

La herramienta mas importante que estaremos instalando en nuestro VPS sera docker. Primero debemos [establecer una conexion con nuestro servidor](./SSH.md#conectandonos-a-nuestro-servidor-usando-ssh) y una vez estemos en el mismo, comenzar con las instalaciones.

## Docker

Una vez tengamos abierta [una terminal correspondiente al sistema operativo de nuestro VPS](./SSH.md#resultado), procederemos a instalar docker en el mismo.

### Ubuntu/Debian

# Arquitectura de carpetas recomendada

Como buenas practicas, es recomendado usar la ruta `/srv/...` para guardar tus proyectos, y seguir una estructura organizada y detallada que permita escalabilidad y orden.

### Ejemplo de estructura organizada

```
/srv/https/
    ├──your-domain.com/
    |   ├── your-api-one/
    |   │   ├── my_folder/
    |   │   |   └── image.png
    |   │   ├── dockercompose.yalm
    |   │   └── readme.md
    |   └── your-api-two/
    |       └── src/...
    |
    ├──your-domain2.com/
    |   ├── your-api-one/
    |          ├── my_folder/..
    |          └── ...
        
```

### Por que' usar la ruta `/srv/` para agregar mis proyectos en un vps es una buena practica?

La ruta `/srv/` es una excelente elección para tus proyectos en un VPS por varias razones fundamentales:

**Estándar Universal**

- Es el directorio oficial definido por el Filesystem Hierarchy Standard (FHS)
- Garantiza compatibilidad con futuras actualizaciones del sistema
- Facilita el trabajo con otros administradores y desarrolladores

**Seguridad y Aislamiento**

- Separa los datos del sitio del sistema operativo
- Facilita la configuración de permisos y ACLs
- Permite políticas de seguridad más precisas

**Organización Profesional**

- Jerarquía clara y predecible
- Facilita la gestión de múltiples proyectos
- Simplifica la documentación y mantenimiento

Esta estructura no solo sigue los estándares de la industria, sino que también facilita el mantenimiento profesional de tu VPS , permitiéndote centrarte en el desarrollo de tus proyectos en lugar de preocuparte por la organización de archivos.

# Levantando proyectos

## Crear y correr imagen de Docker

Aqui lo que yo hago es crear el docker compose directamente en el VPS, y usando [`docker-compose up`](./Docker.md#docker-compose) levantarlo para probarlo. Para levantarlo y dejarlo corriendo permanentemente usar:

```shell
TODO: Codido bash que tengo en el VPS
```

## Automatizacion en Github

Usando githun actions automatizamos el proyecto de tal forma que se genere una nueva imagen de docker y se llame a ejecutar el docker compose que tenemos dentro de nuestro VPS. El docker compose tambien se puede copiar desde nuestro proyecto y pegarlo en una direccion de nuestro VPS usando bash y un [`docker-compose`](./Docker.md#docker-compose) en nuestro repositorio. A continuacion se muestra como podemos configurar github actions para levantar una imagen de docker en nuestro servidor.

### Usando contrasena

```yml
name: Develop - Build and Deploy

on:
  push:
    branches: 
      - main 
    tags:
      - 'dev-v*.*.*'
env:
  IMAGE_NAME: mi_image_name
  VPS_WORKING_DIR: /srv/https/your_domain.com/your_project_name/...

jobs:
  build-and-push:
    name: Build and push
    runs-on: ubuntu-latest

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v3
      
      - name: Get the date
        id: date
        run: echo "date=$(date +'%Y%m%d-%H%M%S')" >> $GITHUB_ENV
        
      - name: Login to DockerHub
        uses: docker/login-action@v3
        with:
          username: ${{ secrets.DOCKERHUB_USERNAME }}
          password: ${{ secrets.DOCKERHUB_TOKEN }}

      - name: Build and push to DockerHub
        uses: docker/build-push-action@v5
        with:
          context: .
          push: true
          tags: |
            ${{ secrets.DOCKERHUB_USERNAME }}/${{ env.IMAGE_NAME }}:dev-latest
            ${{ secrets.DOCKERHUB_USERNAME }}/${{ env.IMAGE_NAME }}:dev-${{ env.date }}

  deploy:
    needs: build-and-push
    name: Deploy to VPS
    runs-on: ubuntu-latest

    steps:
      - name: Deploy to dev
        uses: appleboy/ssh-action@v1.0.3
        with:
          host: ${{ secrets.VPS_HOST }}
          username: ${{ secrets.VPS_USERNAME }}
          password: ${{ secrets.VPS_PASSWORD }}
          script: |
            cd ${{ env.VPS_WORKING_DIR }}
            ls -la
            echo "${{ secrets.DOCKERHUB_TOKEN }}" | docker login -u ${{ secrets.DOCKERHUB_USERNAME }} --password-stdin
            docker compose -f docker-compose-dev.yml pull
            docker compose -f docker-compose-dev.yml up -d --force-recreate
```

### Variables

- `secrets.DOCKERHUB_USERNAME`: Guardar tu username de dockerhub en una variable secreta de github actions llamada `DOCKERHUB_USERNAME`.
-

---

### Usando Keys

```yml
name: Develop - Build and Deploy

on:
  push:
    branches: 
      - main 
    tags:
      - 'dev-v*.*.*'
env:
  IMAGE_NAME: mi_image_name
  VPS_WORKING_DIR: /srv/https/your_domain.com/your_project_name/...

jobs:
  build-and-push:
    name: Build and push
    runs-on: ubuntu-latest

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v3
      
      - name: Get the date
        id: date
        run: echo "date=$(date +'%Y%m%d-%H%M%S')" >> $GITHUB_ENV
        
      - name: Login to DockerHub
        uses: docker/login-action@v3
        with:
          username: ${{ secrets.DOCKERHUB_USERNAME }}
          password: ${{ secrets.DOCKERHUB_TOKEN }}

      - name: Build and push to DockerHub
        uses: docker/build-push-action@v5
        with:
          context: .
          push: true
          tags: |
            ${{ secrets.DOCKERHUB_USERNAME }}/${{ env.IMAGE_NAME }}:dev-latest
            ${{ secrets.DOCKERHUB_USERNAME }}/${{ env.IMAGE_NAME }}:dev-${{ env.date }}

  deploy:
    needs: build-and-push
    name: Deploy to VPS
    runs-on: ubuntu-latest

    steps:
      - name: Deploy to VPS
        uses: appleboy/ssh-action@v1.0.3
        with:
          host: ${{ secrets.VPS_HOST }}
          username: ${{ secrets.VPS_USERNAME }}
          key: ${{ secrets.VPS_SSH_KEY }}
          passphrase: ${{ secrets.VPS_SSH_KEY_PASSPHRASE }}
          script: |
            cd ${{ env.VPS_WORKING_DIR }}
            ls -la
            echo "${{ secrets.DOCKERHUB_TOKEN }}" | docker login -u ${{ secrets.DOCKERHUB_USERNAME }} --password-stdin
            docker compose -f docker-compose-dev.yml pull
            docker compose -f docker-compose-dev.yml up -d --force-recreate
```

### Variables Secretas

- `DOCKERHUB_USERNAME`:
- `DOCKERHUB_TOKEN`:
- `VPS_HOST`: La dirección IP o dominio de tu servidor
- `VPS_USERNAME`: El nombre de usuario para SSH
- `VPS_SSH_KEY`: El contenido de tu clave privada SSH
- `VPS_SSH_KEY_PASSPHRASE`: La frase de paso de tu clave SSH. Si tu ssh-key no tiene passphrase, quitar esta linea del `.yml`: `passphrase: ${{ secrets.VPS_SSH_KEY_PASSPHRASE }}`.

## Copiar `docker-compose.yml` desde github

Para copiar nuestro archivo `.yml` hacia nuestro vps es necesario agregar otros pasos a nuestro `.yml`  del workflow. Para ello debes agregar estos antes de hacer `name: Deploy to VPS`.

``` yml
  steps:
      - name: Checkout repository
        uses: actions/checkout@v2
      - name: Copy dist to VPS
        uses: appleboy/scp-action@master
        with:
          host: ${{ secrets.VPS_HOST }}
          username: ${{ secrets.VPS_USERNAME }}
          key: ${{ secrets.VPS_SSH_KEY }}
          become: true  # Activa sudo
          # Selecting .yml"s
          source: docker-compose-dev.yml
          # The path is based on the directory where the user logged into the server starts
          target: ${{ env.VPS_WORKING_DIR }}
      
      # Aqui continua el codigo anterior
      - name: Deploy to VPS ...
```

### Advertencia

- Si usas esta opcion de copiar archivo. Es obligatorio que el archivo se ecuentre en tu repositorio, de lo contrario, fallara el workflow de github actions. En este caso el archivo se llama `docker-compose-dev.yml`. El nombre `docker-compose.yml` no debes usarlo, esta reservado para hacer build y push hacia dockerhub.

- Los archivos de riesgo, se recomienda copiarlos o crearlos directamente, de forma manual, en el VPS, se podria automatizar con github actions pero para ello tendrias que subir archvos de riesgo que quieres automatizar como es `.env`. Si tu repositorio es privado y tienes absoluta seguridad de sus restricciones de seguridad, podrias automatizar este proceso de subir archivos de riesgo, pero esto es opcional segun tus necesidades.
