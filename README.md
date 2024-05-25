# Ansible role [roundcubemail](https://galaxy.ansible.com/ui/standalone/roles/buluma/roundcubemail/documentation)

Install and configure roundcubemail on your system.

|GitHub|Version|Issues|Pull Requests|Downloads|
|------|-------|------|-------------|---------|
|[![github](https://github.com/buluma/ansible-role-roundcubemail/actions/workflows/molecule.yml/badge.svg)](https://github.com/buluma/ansible-role-roundcubemail/actions/workflows/molecule.yml)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-roundcubemail.svg)](https://github.com/buluma/ansible-role-roundcubemail/releases/)|[![Issues](https://img.shields.io/github/issues/buluma/ansible-role-roundcubemail.svg)](https://github.com/buluma/ansible-role-roundcubemail/issues/)|[![PullRequests](https://img.shields.io/github/issues-pr-closed-raw/buluma/ansible-role-roundcubemail.svg)](https://github.com/buluma/ansible-role-roundcubemail/pulls/)|[![Ansible Role](https://img.shields.io/ansible/role/d/buluma/roundcubemail)](https://galaxy.ansible.com/ui/standalone/roles/buluma/roundcubemail/documentation)|

## [Example Playbook](#example-playbook)

This example is taken from [`molecule/default/converge.yml`](https://github.com/buluma/ansible-role-roundcubemail/blob/master/molecule/default/converge.yml) and is tested on each push, pull request and release.

```yaml
---
- name: Converge
  hosts: all
  become: true
  gather_facts: true

  vars_files:
    ../../vars/main.yml

  pre_tasks:
    - name: Update apt cache.
      apt: update_cache=yes cache_valid_time=600
      when: ansible_os_family == 'Debian'
      changed_when: false

  roles:
    - role: buluma.httpd
      httpd_vhosts:
        - name: docroot
          servername: roundcubemail.example.com
          documentroot: "{{ roundcubemail_install_directory }}"
    - role: buluma.roundcubemail
```

The machine needs to be prepared. In CI this is done using [`molecule/default/prepare.yml`](https://github.com/buluma/ansible-role-roundcubemail/blob/master/molecule/default/prepare.yml):

```yaml
---
- name: Prepare
  hosts: all
  become: true
  gather_facts: false

  roles:
    - role: buluma.bootstrap
    - role: buluma.epel
    - role: buluma.buildtools
    - role: buluma.python_pip
    - role: buluma.openssl
      openssl_items:
        - name: apache-httpd
          common_name: "{{ ansible_fqdn }}"
    - role: buluma.selinux
    - role: buluma.httpd
    - role: buluma.php
      php_upload_max_filesize: 5M
      php_post_max_size: 6M
      php_date_timezone: Europe/Amsterdam
      php_extension:
        - mcrypt.so
    - role: buluma.mysql
      mysql_databases:
        - name: roundcube
      mysql_users:
        - name: roundcube
          password: roundcube
          priv: "roundcube.*:ALL"
```

Also see a [full explanation and example](https://buluma.github.io/how-to-use-these-roles.html) on how to use these roles.

## [Role Variables](#role-variables)

The default values for the variables are set in [`defaults/main.yml`](https://github.com/buluma/ansible-role-roundcubemail/blob/master/defaults/main.yml):

```yaml
---
# defaults file for roundcubemail

roundcubemail_database_host: localhost
roundcubemail_database_user: roundcube
roundcubemail_database_password: roundcube
roundcubemail_database_name: roundcube

# A URL to get support.
roundcubemail_support_url: "{{ ansible_fqdn }}/support"

# A key to encrypt sensitive data.
roundcubemail_des_key: 964af56991531a805bd55085

# The spellchecker to use. Either: 'google', 'pspell', 'enchant' or 'atd'.
roundcubemail_spellcheck_engine: pspell

# The mail host chosen to perform the log-in.
roundcubemail_default_host: localhost
roundcubemail_default_port: 143

# SMTP server host (for sending mails).
roundcubemail_smtp_server: localhost
roundcubemail_smtp_port: 25
roundcubemail_smtp_user: ""
roundcubemail_smtp_pass: ""
```

## [Requirements](#requirements)

- pip packages listed in [requirements.txt](https://github.com/buluma/ansible-role-roundcubemail/blob/master/requirements.txt).

## [State of used roles](#state-of-used-roles)

The following roles are used to prepare a system. You can prepare your system in another way.

| Requirement | GitHub | Version |
|-------------|--------|--------|
|[buluma.bootstrap](https://galaxy.ansible.com/buluma/bootstrap)|[![Ansible Molecule](https://github.com/buluma/ansible-role-bootstrap/actions/workflows/molecule.yml/badge.svg)](https://github.com/buluma/ansible-role-bootstrap/actions/workflows/molecule.yml)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-bootstrap.svg)](https://github.com/shadowwalker/ansible-role-bootstrap)|
|[buluma.buildtools](https://galaxy.ansible.com/buluma/buildtools)|[![Ansible Molecule](https://github.com/buluma/ansible-role-buildtools/actions/workflows/molecule.yml/badge.svg)](https://github.com/buluma/ansible-role-buildtools/actions/workflows/molecule.yml)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-buildtools.svg)](https://github.com/shadowwalker/ansible-role-buildtools)|
|[buluma.epel](https://galaxy.ansible.com/buluma/epel)|[![Ansible Molecule](https://github.com/buluma/ansible-role-epel/actions/workflows/molecule.yml/badge.svg)](https://github.com/buluma/ansible-role-epel/actions/workflows/molecule.yml)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-epel.svg)](https://github.com/shadowwalker/ansible-role-epel)|
|[buluma.httpd](https://galaxy.ansible.com/buluma/httpd)|[![Ansible Molecule](https://github.com/buluma/ansible-role-httpd/actions/workflows/molecule.yml/badge.svg)](https://github.com/buluma/ansible-role-httpd/actions/workflows/molecule.yml)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-httpd.svg)](https://github.com/shadowwalker/ansible-role-httpd)|
|[buluma.mysql](https://galaxy.ansible.com/buluma/mysql)|[![Ansible Molecule](https://github.com/buluma/ansible-role-mysql/actions/workflows/molecule.yml/badge.svg)](https://github.com/buluma/ansible-role-mysql/actions/workflows/molecule.yml)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-mysql.svg)](https://github.com/shadowwalker/ansible-role-mysql)|
|[buluma.ca_certificates](https://galaxy.ansible.com/buluma/ca_certificates)|[![Ansible Molecule](https://github.com/buluma/ansible-role-ca_certificates/actions/workflows/molecule.yml/badge.svg)](https://github.com/buluma/ansible-role-ca_certificates/actions/workflows/molecule.yml)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-ca_certificates.svg)](https://github.com/shadowwalker/ansible-role-ca_certificates)|
|[buluma.openssl](https://galaxy.ansible.com/buluma/openssl)|[![Ansible Molecule](https://github.com/buluma/ansible-role-openssl/actions/workflows/molecule.yml/badge.svg)](https://github.com/buluma/ansible-role-openssl/actions/workflows/molecule.yml)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-openssl.svg)](https://github.com/shadowwalker/ansible-role-openssl)|
|[buluma.php](https://galaxy.ansible.com/buluma/php)|[![Ansible Molecule](https://github.com/buluma/ansible-role-php/actions/workflows/molecule.yml/badge.svg)](https://github.com/buluma/ansible-role-php/actions/workflows/molecule.yml)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-php.svg)](https://github.com/shadowwalker/ansible-role-php)|
|[buluma.python_pip](https://galaxy.ansible.com/buluma/python_pip)|[![Ansible Molecule](https://github.com/buluma/ansible-role-python_pip/actions/workflows/molecule.yml/badge.svg)](https://github.com/buluma/ansible-role-python_pip/actions/workflows/molecule.yml)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-python_pip.svg)](https://github.com/shadowwalker/ansible-role-python_pip)|
|[buluma.reboot](https://galaxy.ansible.com/buluma/reboot)|[![Ansible Molecule](https://github.com/buluma/ansible-role-reboot/actions/workflows/molecule.yml/badge.svg)](https://github.com/buluma/ansible-role-reboot/actions/workflows/molecule.yml)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-reboot.svg)](https://github.com/shadowwalker/ansible-role-reboot)|
|[buluma.selinux](https://galaxy.ansible.com/buluma/selinux)|[![Ansible Molecule](https://github.com/buluma/ansible-role-selinux/actions/workflows/molecule.yml/badge.svg)](https://github.com/buluma/ansible-role-selinux/actions/workflows/molecule.yml)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-selinux.svg)](https://github.com/shadowwalker/ansible-role-selinux)|

## [Dependencies](#dependencies)

Most roles require some kind of preparation, this is done in `molecule/default/prepare.yml`. This role has a "hard" dependency on the following roles:

- {'role': 'buluma.httpd'}

## [Context](#context)

This role is a part of many compatible roles. Have a look at [the documentation of these roles](https://buluma.github.io/) for further information.

Here is an overview of related roles:

![dependencies](https://raw.githubusercontent.com/buluma/ansible-role-roundcubemail/png/requirements.png "Dependencies")

## [Compatibility](#compatibility)

This role has been tested on these [container images](https://hub.docker.com/u/buluma):

|container|tags|
|---------|----|
|[Debian](https://hub.docker.com/r/buluma/debian)|all|
|[Fedora](https://hub.docker.com/r/buluma/fedora)|all|
|[Ubuntu](https://hub.docker.com/r/buluma/ubuntu)|jammy|
|[Kali](https://hub.docker.com/r/buluma/kali)|all|

The minimum version of Ansible required is 2.12, tests have been done to:

- The previous version.
- The current version.
- The development version.

If you find issues, please register them in [GitHub](https://github.com/buluma/ansible-role-roundcubemail/issues)

## [Changelog](#changelog)

[Role History](https://github.com/buluma/ansible-role-roundcubemail/blob/master/CHANGELOG.md)

## [License](#license)

[Apache-2.0](https://github.com/buluma/ansible-role-roundcubemail/blob/master/LICENSE)

## [Author Information](#author-information)

[Shadow Walker](https://buluma.github.io/)
