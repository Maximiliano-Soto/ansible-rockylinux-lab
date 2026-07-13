# Laboratorio 1 - Automatización de servidores Linux con Ansible

## Descripción

Este laboratorio tiene como objetivo implementar un entorno de automatización utilizando Ansible sobre Rocky Linux 8.6.

El proyecto contempla la creación de un nodo controlador y un nodo administrado para automatizar tareas de administración de sistemas, tales como:

- Creación de usuarios.
- Instalación de paquetes.
- Configuración del servicio SSH.
- Configuración del firewall.
- Despliegue de un servidor web.

El laboratorio será desarrollado paso a paso y documentado completamente, incluyendo capturas de pantalla y explicaciones técnicas.

## Tecnologías

- Rocky Linux 8.6 Minimal
- Ansible
- VirtualBox
- Git
- GitHub

---

## Creación del entorno de laboratorio

Se implementó un entorno de virtualización utilizando VirtualBox compuesto por dos máquinas virtuales:

| Máquina | Función |
|---------|---------|
| ansible-controller | Nodo controlador de Ansible |
| webserver01 | Nodo administrado |

### Especificaciones

- Sistema Operativo: Rocky Linux 8.6 Minimal
- CPU: 2 vCPU
- Memoria RAM: 2 GB
- Disco: 25 GB (dinámico)

Cada máquina fue configurada con dos interfaces de red:

- Adaptador NAT para acceso a Internet.
- Adaptador Red Interna para la comunicación entre ambos equipos.