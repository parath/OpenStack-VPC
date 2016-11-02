# Openstack Keystone role

## Installs
 - Keystone
 - Keystone-client

## Supports
 - Centos 7

## Playbook example

```yaml
- name: Keystone installation
  hosts: controllers
  user: root
  roles:
    - { role: "openrc", tags: [ "openrc-role" ] }
    - { role: "keystone", tags: [ "keystone-role" ] }

```
