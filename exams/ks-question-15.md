# Question 15 - Events

Events command:
```
kubectl get events -A --sort-by="metadata.creationTimestamp"
```

Delete pod:
```
kubectl delete pod <POD_NAME> -n kube-system
```

> NOTE: first check the correct pod by `kubectl get pods -n kube-system -o wide | grep -i kube-proxy`

Delete container:
```
ssh <NODE_NAME>
crictl ps
crictl rm <CONTAINER_ID>
```