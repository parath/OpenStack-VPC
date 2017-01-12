# Openstack Upgrade role

## Description:
 - This role upgrades one version of rmitaka to another(Mitaka to Newton)

## Supports:
 - Centos 7

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
