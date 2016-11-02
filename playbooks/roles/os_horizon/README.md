# Openstack Horizon

## Description:
 This role installs and configures the Openstach Dashboard, codenamed horizon.

## Supports:
 - Centos 6
 - Centos 7

## Playbook example

```yaml
- name: Installation and setup of role
  hosts: horizon
  user: root
  roles:
    - { role: "horizon", tags: [ "horizon" ] }

```
