[![Proxmox Logo](https://www.proxmox.com/images/proxmox/Proxmox_logo_standard_hex_400px.png)](https://www.proxmox.com/en/proxmox-mail-gateway)

# Ansible Role - Proxmox Mail Gateway

Role to deploy [Proxmox Mail Gateway](https://www.proxmox.com/en/proxmox-mail-gateway) on a linux server.

[![Proxmox Incoming Processing](https://dl.ansibleguy.net/sw_proxmox_mail_gw/flow.png)](https://pmg.proxmox.com/pmg-docs/pmg-admin-guide.html)


[![Molecule Test Status](https://badges.ansibleguy.net/sw_proxmox_mail_gw.molecule.svg)](https://molecule.readthedocs.io/en/latest/)
[![YamlLint Test Status](https://badges.ansibleguy.net/sw_proxmox_mail_gw.yamllint.svg)](https://yamllint.readthedocs.io/en/stable/)
[![Ansible-Lint Test Status](https://badges.ansibleguy.net/sw_proxmox_mail_gw.ansiblelint.svg)](https://ansible-lint.readthedocs.io/en/latest/)
[![Ansible Galaxy](https://img.shields.io/ansible/role/60005)](https://galaxy.ansible.com/ansibleguy/sw_proxmox_mail_gw)
[![Ansible Galaxy Downloads](https://img.shields.io/badge/dynamic/json?color=blueviolet&label=Galaxy%20Downloads&query=%24.download_count&url=https%3A%2F%2Fgalaxy.ansible.com%2Fapi%2Fv1%2Froles%2F60005%2F%3Fformat%3Djson)](https://galaxy.ansible.com/ansibleguy/sw_proxmox_mail_gw)


**Tested:**
* Debian 11

## Functionality


* **Package installation**
  * Ansible dependencies (_minimal_)
  * Systemd
  * Proxmox Mail Gateway
  * PMG dependencies
    * postgreSQL
    * Postfix
  

* **Configuration**
  * default postgreSQL installation

  * **Default opt-ins**:
    * Nginx => using [THIS Role](https://github.com/ansibleguy/infra_nginx)

  * **Default opt-outs**:
    * Enterprise apt-repository (_[subscription needed](https://www.proxmox.com/en/proxmox-mail-gateway/pricing)_)


## Info

* **Warning:** **IF YOU ARE USING A DEDICATED VM FOR THIS SETUP**:

  You should probably use the [ISO installation process](https://www.proxmox.com/en/downloads/category/proxmox-mail-gateway).

  It might be better supported!


* **Note:** this role currently only supports debian-based systems


* **Note:** Most of the role's functionality can be opted in or out.

  For all available options - see the default-config located in the main defaults-file!


* **Warning:** Not every setting/variable you provide will be checked for validity. Bad config might break the role!


* **Warning:** If you choose to install the nginx web server (_default_) and want to use the [built-in ACME certificate management](https://pmg.proxmox.com/pmg-docs/pmg-admin-guide.html#sysadmin_certificate_management) - you will have to configure 'nginx.plain_site' to 'false'.

  As this 'ACME standalone integration' needs the port 80 to be not in use!


* **Note:** Check out the [nice documentation](https://pmg.proxmox.com/pmg-docs/pmg-admin-guide.html#_features) provided by Proxmox!


* **Warning:** Docker containers ARE NOT SUPPORTED.


* **Info:** PMG's web interface default login is done via PAM/System users.

  Normally, at first, via 'root'.


## Prerequisites

See: [Prerequisites](https://github.com/ansibleguy/sw_proxmox_mail_gw/blob/stable/Prerequisites.md)


## Setup

For this role to work - you must install its dependencies first:

```
ansible-galaxy install -r requirements.yml
```


## Usage

### Config

Define the config as needed:
```yaml
pmg:
  fqdn: 'pmg.template.ansibleguy.net'  # valid, public dns-hostname of your server

  manage:
    webserver: true  # set to false to disable nginx-component

  nginx:  # configure the webserver settings => see: https://github.com/ansibleguy/infra_nginx
    aliases: ['mail-gw.ansibleguy.net']  # additional domains to add to the certificate
    ssl:
      mode: 'letsencrypt'  # or selfsigned/ca
      #  if you use 'selfsigned' or 'ca':
      #    cert:
      #      cn: 'Proxmox Mail Gateway'
      #      org: 'AnsibleGuy'
      #      email: 'pmg@template.ansibleguy.net'
    letsencrypt:
      email: 'pmg@template.ansibleguy.net'
```

Bare minimum example:
```yaml
pmg:
  fqdn: 'pmg.template.ansibleguy.net'
```

Example to use PMG's built-in ACME:
```yaml
pmg:
  fqdn: 'pmg.template.ansibleguy.net'

  nginx:
    aliases: ['mail-gw.ansibleguy.net']
    plain_site: false  # nginx will not bind to port 80
    letsencrypt:
      email: 'pmg@template.ansibleguy.net'
```

Example - if you want to setup postgreSQL manually:
```yaml
pmg:
  fqdn: 'pmg.template.ansibleguy.net'

  manage:
    database: false
```

You might want to use 'ansible-vault' to encrypt your passwords:
```bash
ansible-vault encrypt_string
```

### Execution

Run the playbook:
```bash
ansible-playbook -K -D -i inventory/hosts.yml playbook.yml
```

To debug errors - you can set the 'debug' variable at runtime:
```bash
ansible-playbook -K -D -i inventory/hosts.yml playbook.yml -e debug=yes
```
