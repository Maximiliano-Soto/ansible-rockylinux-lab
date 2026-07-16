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

---

## Configuración de red

Se configuró una dirección IP estática en la interfaz correspondiente a la Red Interna mediante la herramienta 'nmtui'.

| Equipo | Dirección IP |
|---------|--------------|
| ansible-controller | 192.168.100.10/24 |
| webserver01 | 192.168.100.20/24 |

La interfaz NAT permanece configurada mediante DHCP para proporcionar acceso a Internet durante la instalación de paquetes.

## Configuración de acceso SSH

Se creó un usuario dedicado (`ansible`) en ambos servidores para ejecutar las tareas de automatización.

Posteriormente se generó un par de claves SSH utilizando el algoritmo Ed25519 en el nodo controlador y se copió la clave pública al nodo administrado mediante `ssh-copy-id`.

Con esta configuración el controlador puede establecer conexiones SSH sin necesidad de ingresar una contraseña, requisito fundamental para la automatización con Ansible.

Adicionalmente, se configuró el usuario `ansible` con privilegios de `sudo` sin solicitud de contraseña (`NOPASSWD`) mediante un archivo ubicado en `/etc/sudoers.d/`. Esta configuración permite que Ansible ejecute tareas administrativas utilizando la directiva `become: yes` sin intervención manual.

> **Nota:** La configuración `NOPASSWD` se implementó únicamente con fines de automatización dentro de este laboratorio. En entornos de producción debe aplicarse siguiendo las políticas de seguridad de la organización.


### Evidencias

- Creación del usuario `ansible`.
- Servicio `sshd` en ejecución.  
- Acceso SSH inicial mediante contraseña.
- Generación del par de claves SSH (Ed25519)
- Copia de la clave pública con `ssh-copy-id`.
- Acceso SSH sin contraseña.
- Configuración de `sudo` sin contraseña para el usuario `ansible`.

---

### Preparación del nodo controlador

En este módulo se preparó el servidor **ansible-controller** para administrar equipos remotos mediante Ansible.

### Actividades realizadas

- Actualización del sistema.
- Instalación del repositorio EPEL.
- Instalación de Ansible.
- Creación del archivo de inventario `hosts.ini`.

### Verificación de conectividad

Una vez configurado el inventario, se verificó la comunicación entre el nodo controlador y el nodo administrado utilizando el módulo `ping` de Ansible.

La respuesta `SUCCESS` confirmó que el acceso mediante SSH y la configuración del inventario eran correctos, permitiendo la administración remota del servidor.

---

## Automatización de la gestión de usuarios

En este módulo se utilizó el módulo `ansible.builtin.user` para crear automáticamente un usuario en el servidor administrado.

Antes de ejecutar el playbook se verificó que el usuario no existía. Posteriormente se ejecutó el playbook y se confirmó su creación.

Finalmente, se volvió a ejecutar el mismo playbook para comprobar la **idempotencia** de Ansible. Como el usuario ya existía, Ansible no realizó modificaciones adicionales (`changed=0`).

### Evidencias

- Creación del playbook `users.yml`.
- Verificación inicial de que el usuario no existía.
- Primera ejecución del playbook.
- Verificación del usuario creado.
- Segunda ejecución del playbook (idempotencia).

---

## Automatización de la instalación de paquetes

En este módulo se automatizó la instalación de herramientas de administración utilizando el módulo `ansible.builtin.dnf`.

Se verificó inicialmente que los paquetes `htop`, `tmux` y `nmap` no estaban presentes en el servidor administrado. Posteriormente, se ejecutó el playbook para instalarlos y se comprobó su correcta instalación.

Finalmente, se ejecutó nuevamente el mismo playbook para demostrar la idempotencia de Ansible. Como los paquetes ya estaban instalados, no fue necesario realizar modificaciones adicionales (`changed=0`).

### Evidencias

- Actualización del playbook `packages.yml`.
- Verificación del estado inicial de los paquetes.
- Primera ejecución del playbook.
- Verificación de la instalación.
- Segunda ejecución del playbook (idempotencia).