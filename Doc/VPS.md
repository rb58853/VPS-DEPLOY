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
