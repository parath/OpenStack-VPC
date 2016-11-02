# Openstack Glance

## Description:
 - Installs GLance API
 - Install GLance registry
 - Configures file system backend

## Supports:
 - Centos 6
 - Centos 7

## Playbook example

```yaml
- name: Glance installation
  hosts: controllers
  user: root
  roles:
    - { role: "glance", tags: [ "glance" ] }

```
