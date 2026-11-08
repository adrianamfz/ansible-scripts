# Proyecto Ansible

Automatización de servicios en openSUSE 16.0 mediante Ansible.

## Servicios incluidos
- Apache y PHP.
- MariaDB.
- Samba.
- FTP (vsftpd).
- PostgreSQL 18 con conexión SSL.

## Requisitos
- Ansible instalado en el nodo de control.
- Servidor openSUSE 16.0.
- Conexión SSH.
- Usuario con permisos sudo.
- Variables protegidas mediante Ansible Vault.

## Estructura
- ansible.cfg: configuración de Ansible.
- inventory/: inventario de servidores.
- playbooks/: archivos de automatización.
- roles/: tareas organizadas por servicio.

## Ejecución

Verificar conexión:

    ansible servidor -m ping -k -K --ask-vault-pass

Comprobar sintaxis:

    ansible-playbook playbooks/deploy.yml --syntax-check --ask-vault-pass

Ejecutar todos los servicios:

    ansible-playbook playbooks/deploy.yml -k -K --ask-vault-pass

Ejecutar PostgreSQL individualmente:

    ansible-playbook playbooks/postgresql.yml -k -K --ask-vault-pass

## Verificación

Comprobar servicios:

    systemctl is-active apache2 mariadb smb vsftpd postgresql

Verificar PostgreSQL:

    psql --version

## Repositorio GitHub

https://github.com/adrianamfz/ansible-scripts
