### Basics
#### Get basic status
```
pcs status
```
#### get more detailed status
```
pcs status --full
```
#### allow cluster to start resource
```
pcs resource enable
```
#### stop resource if running and forbid from starting again
```
pcs resource disable
```
#### restart resource optionally on another node. For example: `r_web_php-fpm`
```
pcs resource restart [node]
```
#### move resource to another node
```
pcs resource move
```
#### show all resources
```
pcs resource show
```
#### show all resources in more detail
```
pcs resource show --full
```
#### show a specific resources details
```
pcs resource show
```
#### cleanup errors, forget about history of failures
```
pcs resource cleanup
```
### Adding resources
#### Add a VIP
```
pcs resource create Cluster_VIP ocf:heartbeat:IPaddr2 ip=10.0.0.44 cidr_netmask=24 op monitor interval=20s
```
#### Enable the resource
```
pcs resource enable Cluster_VIP
```
#### Moving resources
```
pcs resource move [destination node]
```
### Constraints
#### to list current constraints
```
pcs constraint show --full
```
#### remove a constraint, useful for bad location constraints
```
pcs constraint remove
```
#### tell website and VIP to always be on the same node
```
pcs constraint colocation add Website with ClusterIP INFINITY
```
#### tell the VIP to start before the webservice
```
pcs constraint order ClusterIP then Website
```
#### tell a service to prefer a node
```
pcs constraint location Website prefers node1=50
```
### Cluster setup
#### Install the packages
```
yum -y install pacemaker pcs
```
#### start and enable service
```
systemctl start pcsd.service; systemctl enable pcsd.service
```
#### Set credentials for auth
```
passwd hacluster
```
#### Allow the nodes to talk to pcs
```
pcs cluster auth ha-training-1 ha-training-2
```
#### Create the cluster and add nodes
```
pcs cluster setup --name hatraining ha-training-1 ha-training-2
```
#### Disable fencing (one node)
```
pcs property set stonith-enabled=false
```
#### Start and enable corosync
```
systemctl enable corosync.service; systemctl enable corosync.service
```
#### Start all cluster services
```
pcs cluster start --all
```
