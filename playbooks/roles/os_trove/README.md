# Openstack Trove

## Description:
 - This role installs and configures trove components for Openstack Mitaka	

## Supports:
 - Centos 6
 - Centos 7

## Playbook example

```yaml
- name: Installation and setup of role
  hosts: controllers
  user: root
  roles:
    - { role: "os_trove", tags: [ "trove" ] }
  vars:
    variable: "{{ some_variable }}"

```
