
# PROYECTO: Tarea 3 y 4 - Automatización con Ansible y GitHub

**Sistema operativo objetivo:** openSUSE 16.0

## a) Repositorio de GitHub

Repositorio original del proyecto:  
https://github.com/LuzRodriguez2004/Tarea-Ansible

## b) Pasos para ejecutar el proyecto

### 1. Instalar Ansible en el nodo de control

```bash
sudo zypper install ansible
```

Instalar las colecciones requeridas:

```bash
ansible-galaxy collection install community.general community.mysql community.postgresql ansible.posix
```

### 2. Configurar el acceso SSH

Generar una llave SSH:

```bash
ssh-keygen -t ed25519 -f ~/.ssh/id_ansible -C "ansible-control"
```

Copiar la llave pública al servidor destino:

```bash
ssh-copy-id -i ~/.ssh/id_ansible.pub publica@IP_DEL_SERVIDOR
```

### 3. Configurar el inventario

Editar `inventory.ini` y establecer la dirección IP correspondiente al servidor openSUSE:

```ini
[opensuse_servers]
servidor_opensuse ansible_host=IP_DEL_SERVIDOR ansible_user=publica
```

No incluir contraseñas en el inventario.

### 4. Configurar las credenciales

Las variables sensibles se gestionan mediante Ansible Vault.

Configurar las credenciales localmente siguiendo las instrucciones del proyecto. No publicar archivos de credenciales ni contraseñas.

### 5. Verificar conectividad

```bash
ansible opensuse_servers -i inventory.ini -m ping --ask-vault-pass
```

### 6. Validar la sintaxis

```bash
ansible-playbook -i inventory.ini site.yml --syntax-check --ask-vault-pass
```

### 7. Ejecutar el aprovisionamiento

```bash
ansible-playbook -i inventory.ini site.yml --ask-vault-pass
```

## c) Verificación de servicios

Comprobar que los servicios estén activos:

```bash
ansible opensuse_servers -i inventory.ini -b --ask-vault-pass -m command -a "systemctl is-active apache2 mariadb smb nmb vsftpd postgresql"
```

### Apache, PHP y phpMyAdmin

Comprobar Apache:

```bash
curl -I http://IP_DEL_SERVIDOR/
```

Comprobar phpMyAdmin:

```bash
curl -I http://IP_DEL_SERVIDOR/phpMyAdmin/
```

### Samba

Verificar los recursos compartidos:

```bash
smbclient -L //IP_DEL_SERVIDOR -U colab_samba
```

Introducir la contraseña cuando el cliente la solicite.

### FTP

Verificar el servicio:

```bash
systemctl is-active vsftpd
```

### PostgreSQL 18

Comprobar la versión:

```bash
psql --version
```

La conexión remota se configura mediante SSL y las credenciales de la aplicación.

## d) Estructura de roles implementados

- **roles/lamp:** instalación y configuración de Apache, PHP, MariaDB y phpMyAdmin.
- **roles/samba:** instalación y configuración de Samba, creación de usuario, directorio compartido y permisos.
- **roles/ftp:** instalación y configuración de vsftpd, acceso local, jaula chroot y puertos pasivos.
- **roles/postgresql:** instalación y configuración de PostgreSQL 18, base de datos, acceso remoto y conexión SSL.

## e) Configuración y optimización

El proyecto utiliza Ansible para automatizar el aprovisionamiento de servicios mediante roles, plantillas Jinja2, inventario y variables protegidas con Ansible Vault.

## Repositorio personal de publicación

https://github.com/adrianamfz/ansible-scripts
