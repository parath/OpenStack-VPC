# Role Name

## Description:
 - Installs service foo

## Supports:
 - Centos 6
 - Centos 7
 - Ubuntu 14.04
 - Ubuntu 16.04

## Playbook example

```yaml
- name: Installation and setup of role
  hosts: role_hosts
  user: root
  roles:
    - { role: "role", tags: [ "role" ] }
  vars:
    variable: "{{ some_variable }}"

```
