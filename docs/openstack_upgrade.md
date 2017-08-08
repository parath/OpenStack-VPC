# Upgrade Mitaka to Newton

## Following the steps of role os_upgrade which is basically the steps from RDO upgrade doc.
Did upgrade all the services one by one and downtime should have been minimal to none for client load.

## Errors/Hicups
###During upgrade

#### when ```cinder-manage db sync``` failed

from os_brick import encryptors
ImportError: cannot import name encryptors

I install yum install python2-os-brick


#### when ```su -s /bin/sh -c "neutron-db-manage upgrade heads" neutron``` failed
```
File "/usr/lib/python2.7/site-packages/pecan/util.py", line 43, in <lambda>
    key=lambda c: 'self' in c.cell_contents.__code__.co_varnames,
```
I installed yum install python2-pecan

###POST upgrade
#### When cinder create failed with:

```
2017-08-07 18:30:33.979 923 ERROR cinder.api.middleware.fault   File "/usr/lib/python2.7/site-packages/taskflow/engines/action_engine/engine.py", line 80, in wrapper
2017-08-07 18:30:33.979 923 ERROR cinder.api.middleware.fault     return meth(self, *args, **kwargs)
2017-08-07 18:30:33.979 923 ERROR cinder.api.middleware.fault   File "/usr/lib/python2.7/site-packages/taskflow/engines/action_engine/engine.py", line 409, in validate
2017-08-07 18:30:33.979 923 ERROR cinder.api.middleware.fault     atom.name, atom.revert_rebind,
2017-08-07 18:30:33.979 923 ERROR cinder.api.middleware.fault AttributeError: 'QuotaReserveTask' object has no attribute 'revert_rebind'
2017-08-07 18:30:33.979 923 ERROR cinder.api.middleware.fault 
```

the isssue was resolved with openstack services restart, propably need to load modules that did exist or reload modules that were upgraded.


## The nova console could not validate tokens due to memcache config changes from mitaka to newton.
This should be changed...:
```
[DEFAULT]
memcached_servers = ip:port under 
```

to this...:
```
[cache]
enabled = true
backend = oslo_cache.memcache_pool
memcache_servers = {list of memcached servers to connect} # e.g. 192.168.0.6:11211, 192.168.0.7:11211, 192.168.0.7:11211
```

Note that memcached_servers has been changed to memcache*_servers.
