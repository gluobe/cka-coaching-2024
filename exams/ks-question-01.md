# Question 01 - Cluster Contexts

Get all cluster context **names**:
```
kubectl config get-contexts -o name
```

Get current context:
```
kubectl config current-context
```

Get current context without using `kubectl`:
```
grep current-context ~/.kube/config | cut -f2 -d " "
```