Role Name
=========

Ansible role for nginx service with basic authorization to be in front of WEB UI of services: consul, prometheus,
grafana, kibana. Tested on:
 - Ubuntu 14.04 Trusty
 - Ubuntu 16.04 Xenial

Requirements
------------

Installed OS:
 - Ubuntu 14.04 Trusty
 - Ubuntu 16.04 Xenial

Role Variables
--------------

- PASSWD_FILE: /etc/nginx/.htpasswd # Default password encrypted file
- NGINX_CONFIG_FILE: /etc/nginx/sites-available/default # Default ngnix site config

**All ports are mirrored inside from outside, it can be overwritten after deployment by editing
`/etc/nginx/sites-available/default`**

- CONSUL_PORT: 8500 # Default consul port
- PROM_PORT: 9090 # Default prometheus port
- KIBANA_PORT: 5601 # Default kibana port
- GRAFANA_PORT: 3000 # Default grafana port

**Default username and password, can be changed after deployment with `htpasswd` utility.**

- NGINX_DEFAULT_USER: admin
- NGINX_DEFAULT_PASSWD: admin

Example Playbook
----------------

To use this role:

  - create folder (in user $HOME folder in example below) and install role from ansible-galaxy

```bash
cd ~/
mkdir nginx
cd nginx
ansible-galaxy install winmasta.nginx --roles-path .
```

  - create file `hosts`, containing hostname(s) or IP address(es) of host(s), where you want to deploy nginx

```bash
echo "ENTER HOSTNAME OR IP" > hosts
```

  - create file `ansible.cfg` in current folder

```bash
cat > ansible.cfg << EOF
[defaults]
remote_user = root
host_key_checking = False
EOF
```

  - create playbook in current folder `main.yml` with content

```bash
cat > main.yml << EOF
---
- hosts: all
  gather_facts: no

  pre_tasks:

  - name: Install required packages
    raw: sudo apt-get update -y && sudo apt-get -y install python-simplejson python-pip
    changed_when: False

  - setup:

  roles:
    - winmasta.nginx
EOF
```

  - execute playbook `main.yml`

```bash
ansible-playbook -i hosts main.yml
```

License
-------

MIT
