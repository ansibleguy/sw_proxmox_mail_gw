[![LOGOTEXT](URL-to-LOGO)](COMPANY-WEB)

# Ansible Role - SOMETHING

Role to deploy SOMETHING

# REPLACE: GALAXY_ID & ROLE

[![Molecule Test Status](https://badges.ansibleguy.net/ROLE.molecule.svg)](https://molecule.readthedocs.io/en/latest/)
[![YamlLint Test Status](https://badges.ansibleguy.net/ROLE.yamllint.svg)](https://yamllint.readthedocs.io/en/stable/)
[![Ansible-Lint Test Status](https://badges.ansibleguy.net/ROLE.ansiblelint.svg)](https://ansible-lint.readthedocs.io/en/latest/)
[![Ansible Galaxy](https://img.shields.io/ansible/role/GALAXY_ID)](https://galaxy.ansible.com/ansibleguy/ROLE)
[![Ansible Galaxy Downloads](https://img.shields.io/badge/dynamic/json?color=blueviolet&label=Galaxy%20Downloads&query=%24.download_count&url=https%3A%2F%2Fgalaxy.ansible.com%2Fapi%2Fv1%2Froles%2FGALAXY_ID%2F%3Fformat%3Djson)](https://galaxy.ansible.com/ansibleguy/ROLE)


**Tested:**
* Debian 11

## Functionality

* **Package installation**
  * Ansible dependencies (_minimal_)


* **Configuration**
  * 


  * **Default config**:
    * 
 

  * **Default opt-ins**:
    * 


  * **Default opt-outs**:
    * 


## Info

* **Note:** this role currently only supports debian-based systems


* **Note:** Most of the role's functionality can be opted in or out.

  For all available options - see the default-config located in the main defaults-file!


* **Warning:** Not every setting/variable you provide will be checked for validity. Bad config might break the role!


## Setup

For this role to work - you must install its dependencies first:

```
ansible-galaxy install -r requirements.yml
```


## Usage

### Config

Define the config as needed:

```yaml
app:

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

There are also some useful **tags** available:
* 
*

To debug errors - you can set the 'debug' variable at runtime:
```bash
ansible-playbook -K -D -i inventory/hosts.yml playbook.yml -e debug=yes
```
