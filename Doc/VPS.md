# VPS (Servidor Privado Virtual)

## Overview

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

## Instalacion de bibliotecas y herramientas

La herramienta mas importante que estaremos instalando en nuestro VPS sera docker. Primero debemos [establecer una conexion con nuestro servidor](./SSH.md#conectandonos-a-nuestro-servidor-usando-ssh) y una vez estemos en el mismo, comenzar con las instalaciones.

### Docker

Una vez este abierto nuestro VPS procederemos a instalar docker en el mismo.

#### Ubuntu/Debian
