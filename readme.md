# Implementaciones de Servidores Virtuales Privados (VPS)

Este repositorio documenta sistemáticamente experiencias técnicas y mejores prácticas para el despliegue de infraestructuras en entornos VPS, así como en ambientes locales de prueba.

## Protocolo Secure Shell (SSH)

`SSH (Secure Shell)` constituye un protocolo de comunicación remota criptográficamente seguro diseñado para facilitar el acceso y gestión administrativa de sistemas informáticos distribuidos. Proporciona un canal de comunicación cifrado que garantiza la integridad y confidencialidad de las sesiones de administración remota.

### Arquitectura Técnica

Los componentes fundamentales del protocolo SSH incluyen:

- **Cliente SSH:** Interfaz de usuario que establece y mantiene la conexión segura (implementada mediante comandos como
 en sistemas Unix/Linux)
- **Servidor SSH:** Demonió (
) responsable del procesamiento de solicitudes de conexión y autenticación
- **Sistema de Claves de Autenticación:** Mecanismo de seguridad basado en pares de claves pública/privada para verificar identidades
- **Protocolo de Cifrado:** Conjunto de algoritmos avanzados que aseguran la privacidad y autenticidad de los datos transmitidos

[Ver casos de uso y pasos para algunas conexiones SSH con nuestro VPS]((/Doc/SSH.md))

## Contenerización con Docker y Despliegue en VPS

`Docker` representa una plataforma especializada en contenerización que permite encapsular aplicaciones completas junto con sus dependencias y configuraciones específicas. Esta arquitectura proporciona un entorno consistente y portable, optimizando la eficiencia de recursos y simplificando los procesos de desarrollo y despliegue.

[Ver casos de uso de docker sobre la base de montar imagenes en servidores.]((/Doc/Docker.md))

# VPS (Servidor Privado Virtual)

Un VPS es un entorno virtual aislado que funciona como un servidor dedicado dentro de un servidor físico compartido
. Es como tener una habitación privada en un restaurante, donde tienes control total sobre tu espacio aunque compartas el edificio con otros
.

[Ver usos, despliegues y arquitecturas con buenas practicas en VPS](./Doc/VPS.md)
