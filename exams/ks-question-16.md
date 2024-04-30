# Question 16 - Namespace

Create namespace:
```
kubectl create namespace cka-master
```

All namespaced resources:
```
kubectl api-resources --namespaced -o name
```

Namespace with most roles:
```
kubectl get roles -A
```

Or a bit more automated:
```
kubectl get roles -A --no-headers | awk '{print $1} | sort | uniq -c
```