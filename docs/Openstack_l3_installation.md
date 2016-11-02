# Install and configure Neutron l3 agent on existing openstack installation
# The document describes both a DVR (Distributed Virtual Router) and nor DVR enabled installation
# Any existing workload should be deleted and recreated in order to activate and consume neutron l3 capabilities
# Alternatively you can just detach the interfaces of the provider network from the vms that is attached and recreate the network marked as external.

# NON DVR
# PACKAGES INSTALLATION
## Install openstack-neutron-l3 on on one controler (for NON DVR mode)

neutron-l3 agent is already included in openstack-neutron package

# CONFIGURATION FILES
## Include the following on all controller nodes in /etc/neutron/l3_agent.ini

[DEFAULT]
interface_driver = neutron.agent.linux.interface.OVSInterfaceDriver
external_network_bridge =
agent_mode = legacy
enable_metadata_proxy = True

## Include the following on the controller nodes in /etc/neutron/neutron.conf
[DEFAULT]
service_plugins = router




# DVR
# PACKAGES INSTALLATION
## INstall openstack-neutron-l3 on on one controler and on all compute nodes (for DVR mode)
1) Install neutron-l3-agent package on all controllers and compute nodes

neutron-l3 agent is already included in openstack-neutron package

## Controller nodes
## Include the following on the Network node /etc/neutron/l3_agent.ini  (can be one of the controllers)
[DEFAULT]
interface_driver = neutron.agent.linux.interface.OVSInterfaceDriver
external_network_bridge =
agent_mode = dvr_snat

## Include the following on the controllers /etc/neutron/neutron.conf 
[DEFAULT]
service_plugins = router
router_distributed =  True

## Compute nodes
## Include the following on the compute nodes /etc/neutron/l3_agent.ini
[DEFAULT]
interface_driver = neutron.agent.linux.interface.OVSInterfaceDriver
external_network_bridge =
agent_mode = dvr
enable_metadata_proxy = True

## Include the following on all nodes running neutron-openvwsitch-agnet in /etc/neutron/plugins/ml2/ml2_conf.ini
[agent]
enable_distributed_routing =  True
