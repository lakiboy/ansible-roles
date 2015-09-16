# Ansible roles

Basic ansible roles to setup LEMP stack on Ubuntu.

## Usage

_site.yml_:

```yaml
---
- hosts: webservers
  remote_user: root

  pre_tasks:
    - name: Setting hostname
      hostname: "name={{ hostname }}"

  roles:
    - users
    - mariadb
    - php
    - nginx
    - devtools
```

_hosts_:

```plain
[webservers]
app.com hostname=app
```

_host_vars/app.com.yml_:
```yaml
---
nginx_servers_symfony:
  - name:   app
    host:   www.app.com
    path:   /var/www/app
    target: /prod/current/web
    owner:  deploy
    group:  deploy
    aliases:
      - app.com

mariadb_databases:
  - app

mariadb_users:
  - name: app
    priv: "*.*:ALL"
    pass: app

devtools_github_user_tokens:
  - user:  deploy
    token: %token%
```

Running playbook:

```bash
$ ansible-playbook -i hosts site.yml
```
