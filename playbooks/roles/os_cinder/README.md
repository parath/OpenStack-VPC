# Openstack cinder

## Description:
 - This role installs and configures Openstack Cinder blovk storage service
 - Install cinder-scheduler cinder-api cinder-volume

## Supports:
 - Centos 6
 - Centos 7

## Playbook example

```yaml
- name: Installation and setup Openstack Cinder
  hosts: cinder
  user: root
  roles:
    - { role: "cinder", tags: [ "cinder" ] }
  vars:
    variable: "{{ some_variable }}"

```
