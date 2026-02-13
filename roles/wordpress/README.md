# Ansible Role: WordPress

This Ansible role installs and configures a full WordPress stack, including Apache, MariaDB, PHP, and WordPress itself.

## Features

- Installs and configures Apache web server.
- Installs and configures MariaDB database server.
- Creates a dedicated database and user for WordPress.
- Installs PHP and required extensions for WordPress
- Downloads and installs the latest (or specified version of) WordPress.
- keeps database credentials and the WordPress keys and salts in a vault.
- Defines strong random values for all the WordPress keys and salts in an encryped vault.
- Configures WordPress `wp-config.php` with the database credentials and the WordPress keys and salts.
- Sets up Apache virtual host for WordPress.

## Requirements

This role assumes you have a target host(s) with:

- A supported Linux distribution (Ubuntu, Debian).
- Root or sudo privileges for the Ansible user.
- Internet connectivity to download packages and WordPress.

## Role Variables

The following variables can be overridden to customize the installation. Default values are provided in `roles/wordpress/defaults/main.yml`.
- `mariadb_root_password`: (Required) The root password for MariaDB. *Change this from the default!. Use roles/wordpress/vars/vault.yml to keep passwords secure.
- `wordpress_db_name`: Name of the WordPress database. (Default: `wordpress_db`)
- `wordpress_db_user`: Username for the WordPress database. (Default: `wordpress_user`)
- `wordpress_db_password`: (Required) Password for the WordPress database user. Change this from the default!*. Use roles/wordpress/vars/vault.yml to keep passwords secure.
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

    # For changing "mariadb_root_password" and "wordpress_db_password" refer to roles/wordpress/vars/vault.yml "123456Aa@" ;)

    # wordpress_domain: "your-wordpress-site.com"
    
    # اگر نیاز به تغییر نام دیتابیس یا یوزر وردپرس دارید، می‌توانید اینجا اضافه کنید:
    # wordpress_db_name: "my_custom_wp_db"
    # wordpress_db_user: "my_custom_wp_user"

    # مثال برای تنظیم نسخه وردپرس:
    # wordpress_version: "6.2.2"

``` 
## Run command

```bash
ansible-playbook playbook.yml --ask-become-pass --ask-vault-pass
```