# Openstack Mitaka installaton with packages

## Introduction
The present document describes the steps to configure and install Openstack Mitaka on CentOS 7 using the Stackmasters ansible playbooks and roles. 
The installation uses centos-mitaka repository.

### Roles 
 - os_common
 - os_keystone
 - os_glance
 - os_neutron
 - os_nova
 - os_cinder

### Playbooks
 - Install-percona-galera.yml
 - install-rabbitmq.yml
 - install-memcached.yml
 - install-os_common
 - install-os_keystone
 - install-os_glance
 - install-os_neutron
 - install-os_neutron
 - install-os_nova
 - install-os_cinder


## Prerequisites
### Instalation 
 - Operating System to be installed (CentOS 7)
 - Network configuration (specific bridges and interfaces)

### Install the Operating System
Install CentOS 7 minimal and setup ntp and networking

### Create the following bridges
Create br-mgmt, br-tunnel, br-storage and br-ex on all nodes.Bridge br-ex will be used for flat networks and external connectivity to the vms. If you intentd to use OVS as the neutron mechanism, then br-ex bridge should be an OVS type bridge.

####OVS Bridge
```bash
DEVICE="br-ex"
BOOTPROTO="none"
DNS1="DNS_IP"
GATEWAY="GATEWAY_IP"
IPADDR="IP"
NETMASK="NETMASK"
NM_CONTROLLED="no"
DEFROUTE="yes"
IPV4_FAILURE_FATAL="yes"
IPV6INIT=no
ONBOOT="yes"
TYPE="OVSBridge"
DEVICETYPE="ovs"
```
####Linux Bridge
```
DEVICE="br-ex"
BOOTPROTO="none"
DNS1="DNS_IP"
GATEWAY="GATEWAY_IP"
IPADDR="IP"
NETMASK="NETMASK"
NM_CONTROLLED="no"
DEFROUTE="yes"
IPV6INIT=no
ONBOOT="yes"
BOOTPROTO=none
DELAY=0
TYPE="Bridge"
```

## Configuration
### Setup inventory

####Use the inventory/openstack file and fill in controllers, compute nodes and storage nodes as show below. Fill in as many hosts as you like and by defining the hostname and the ansible_ssh_host variable. Then define that host in the specific group that belongs.

```yaml
controll1 ansible_ssh_host=
controll2 ansible_ssh_host=
controll3 ansible_ssh_host=

compute1 ansible_ssh_host=
compute2 ansible_ssh_host=
compute3 ansible_ssh_host=

storage1 ansible_ssh_host=
storage2 ansible_ssh_host=
storage3 ansible_ssh_host=

haproxy1 ansible_ssh_host=

[controllers]
controll1
controll2
controll3

[compute]
compute1
compute2
compute3

[storage]
storage1
storage2
storage3

[haproxy]
haproxy1
```

### Fill in all variables
Use var/openstack.yml file and fill in any variable that is blanc or needs a diferent value

```yaml
##########################
# Shared Infra Variables #
##########################

# HAProxy VIP
internal_vip: 
external_vip: 

# Galera
galera_root_pass: 

## Memcached
memcached_port: 11211
memcached_servers: "{% for host in groups['controllers'] %}{{ hostvars[host]['ansible_br_mgmt']['ipv4']['address'] }}:{{ memcached_port }}{% if not loop.last %},{% endif %}{% endfor %}"

## RabbitMQ
rabbitmq_port: 5672
rabbitmq_hosts: "{% for host in groups['controllers'] %}{{ hostvars[host]['ansible_br_mgmt']['ipv4']['address'] }}:{{ rabbitmq_port }}{% if not loop.last %},{% endif %}{% endfor %}"
# Name of the rabbitmq cluster
rabbitmq_cluster_name: rabbitmq_cluster1

# Erlang authentication cookie, should be automaticaly generated
rabbitmq_cookie_token: 

#######################
# Openstack Variables #
#######################

# Keystone
keystone_admin_user: admin
keystone_admin_user_password: 
keystone_admin_role: admin

## Neutron
# Neutron service details
neutron_service_region: RegionOne
neutron_service_user_name: neutron
neutron_service_user_pass: 
neutron_service_role_name: "admin"
neutron_service_project_domain_id: default
neutron_service_user_domain_id: default
neutron_service_project_name: "service"
neutron_metadata_agent_secret: 

## Nova
nova_service_name: nova
nova_service_region: RegionOne
nova_service_project_name: "service"
nova_service_project_domain_id: default
nova_service_user_domain_id: default
nova_service_user_name: "nova"
nova_service_user_pass: 
nova_service_role_name: "admin"


## Glance
# Glance Database details
glance_db_name: glance
glance_db_pass: 
glance_db_user: glance

# Glance system details
glance_system_user: glance
glance_system_group: glance

# Glance service details
glance_service_user_name: glance
glance_service_user_pass: 
glance_service_role_name: "admin"
glance_service_project_domain_id: default
glance_service_user_domain_id: default
glance_service_project_name: service
glance_service_region: RegionOne

# Glance RabbitMQ info
glance_rabbitmq_userid: glance
glance_rabbitmq_pass: 
glance_rabbitmq_vhost: /glance

# Glance store
glance_default_store: rbd
glance_store: rbd

## Ceph
ceph_uuid: 0c71259f-2cc2-415f-8f4c-416aef2d85f3
ceph_mons: "{% for host in groups['mons'] %}{{ hostvars[host]['ansible_br_torage']['ipv4']['address'] }}{% if not loop.last %},{% endif %}{% endfor %}"

# Glance Ceph properties
glance_ceph_pool: images
glance_ceph_client: glance

# Cinder ceph
cinder_ceph_client: cinder
cinder_ceph_pool: volumes

```

## Run the playbooks
### Infra playbooks
First you need to run the galera playbook from within the playbooks direcory.

```bash
root@ansible# ansible-playbook install-galera.yml -i inventory/openstack -e @vars/openstack.yml
```

```bash
root@ansible# ansible-playbook install-rabbitmq.yml -i inventory/openstack -e @vars/openstack.yml 
```
```bash
root@ansible# ansible-playbook install-memcached.yml -i inventory/openstack -e @vars/openstack.yml
```

```bash
root@ansible# ansible-playbook install-haproxy.yml -i inventory/openstack -e @vars/openstack.yml @vars/configs/haproxy-os-config.yml,
```

### Openstack playbooks

```bash
root@ansible# ansible-playbook install-os_all.yml -i inventory/openstack -e @vars/openstack.yml
```
