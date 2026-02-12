```markdown
# Ansible Role: WordPress

This Ansible role installs and configures a full WordPress stack, including Apache, MariaDB, and WordPress itself.

## Features

- Installs and configures Apache web server.
- Installs and configures MariaDB database server.
- Creates a dedicated database and user for WordPress.
- Downloads and installs the latest (or specified version of) WordPress.
- Configures WordPress `wp-config.php` with database credentials and security salts.
- Sets up Apache virtual host for WordPress.

## Requirements

This role assumes you have a target host(s) with:

- A supported Linux distribution (Ubuntu, Debian, CentOS, RedHat).
- Root or sudo privileges for the Ansible user.
- Internet connectivity to download packages and WordPress.

## Role Variables

The following variables can be overridden to customize the installation. Default values are provided in `roles/wordpress/defaults/main.yml`.

- `mariadb_root_password`: (Required) The root password for MariaDB. *Change this from the default!
- `wordpress_db_name`: Name of the WordPress database. (Default: `wordpress_db`)
- `wordpress_db_user`: Username for the WordPress database. (Default: `wordpress_user`)
- `wordpress_db_password`: (Required) Password for the WordPress database user. Change this from the default!*
- `apache_packages`: List of Apache packages to install. (Default: `['apache2']` for Debian/Ubuntu)
- `mariadb_packages`: List of MariaDB packages to install. (Default: `['mariadb-server', 'mariadb-client']`)
- `wordpress_web_root`: The directory where WordPress files will be installed. (Default: `/var/www/wordpress`)
- `wordpress_domain`: The domain name or IP address to access WordPress. (Default: `localhost`)
- `wordpress_version`: The version of WordPress to download. Use `latest` or a specific version like `6.2.2`. (Default: `latest`)
- `wordpress_owner`: The system user that will own the WordPress files. (Default: `www-data`)
- `wordpress_group`: The system group that will own the WordPress files. (Default: `www-data`)

### Important Variables to Change

- `mariadb_root_password`
- `wordpress_db_password`
- `wordpress_domain` (if not using localhost)

## Example Playbook

```yaml
---
- name: Deploy WordPress Stack
  hosts: webservers # Replace with your target host group
  become: yes # Run tasks with sudo privileges
  roles:
    - wordpress

  vars:
    # --- Customize these variables ---
    mariadb_root_password: "YOUR_STRONG_ROOT_PASSWORD" # !! IMPORTANT: Change this !!
    wordpress_db_password: "YOUR_STRONG_WP_DB_PASSWORD" # !! IMPORTANT: Change this !!
```
## Run sample

```bash
ansible-playbook playbook.yml --ask-become-pass --ask-vault-pass

```

## Note
After first run .secret directory will be created. this directory contains wordpress salts. For security reaseons encrypt them with vault:

```bash

ansible-vault encrypt .secrets/*

```