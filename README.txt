# Stackmasters OpenStack Virtual Private Cloud

## Discription
This repository holds Ansible playbooks used to manage Stackmasters Virtual Private Cloud (VPC) based on OpenStack. Master branch contains the latest code that may be unstable or untested. To use a stable OpenStack release choose a stable-* branch.

## Branches
 - master
 - stable-mitaka

To clone a branch (eg mitaka) do: ``` git clone -n stable-mitaka git@github.com:parath/OpenStack-VPC.git```

## OpenStack core services
 - keystone
 - nova
 - glance
 - neutron
 - heat
 - cinder
 - trove
 - 
 
## OpenStack optional services
 - horizon

Neutron notes
----
Only L2 networking is managed within OpenStack.
Connected networks are
 - internal network
 - public network
