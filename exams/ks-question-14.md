# Question 14 - Cluster information

How many master/worker nodes:
```
kubectl get nodes
```

Service CIDR:
```
kubectl cluster-info dump | grep -i service-cluster-ip
```

Networking plugin:
```
ssh <CONTROL_PLANE_NODE>
ls /etc/cni/net.d/
```

Suffix: `-cluster1-node1`

> NOTE: static pods get hostname as suffix