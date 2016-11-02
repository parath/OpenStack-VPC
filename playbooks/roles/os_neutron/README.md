# Openstack Neutron Role

## Installs
 - openstack-neutron
 - openstack-neutron-ml2
 - openstack-neutron-openvswitch
 - ebtables
 - ipset

## Supports
 - Centos 7

## Playbook example

```yaml
- name: Nova installation
  hosts: neutron_all
  user: root
  roles:
    - { role: "openrc", tags: [ "openrc-role" ] }
    - { role: "neutron", tags: [ "nova-role" ] }

```
