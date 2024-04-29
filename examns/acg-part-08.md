# Create a Networkpolicy That Denies All Access to the Maintenance Pod

Switch to the acgk8s context:
```
kubectl config use-context acgk8s
```

Check the foo namespace:
```
kubectl get pods -n foo
```

Check the maintenance pod's labels:
```
kubectl describe pod maintenance -n foo
```

Create a new YAML file named np-maintenance.yml:
```
vim np-maintenance.yml
```

In the file, paste the following:
```
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: np-maintenance
  namespace: foo
spec:
  podSelector:
    matchLabels:
      app: maintenance
  policyTypes:
  - Ingress
  - Egress
```

Create the NetworkPolicy:
```
kubectl create -f np-maintenance.yml
```

Check the status of the exam objectives:
```
./verify.sh
```

# Create a Networkpolicy That Allows All Pods in the users-backend Namespace to Communicate with Each Other Only on a Specific Port

Label the users-backend namespace:
```
kubectl label namespace users-backend app=users-backend
```

Create a YAML file named np-users-backend-80.yml:
```
vim np-users-backend-80.yml
```

In the file, paste the following:
```
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: np-users-backend-80
  namespace: users-backend
spec:
  podSelector: {}
  policyTypes:
  - Ingress
  ingress:
  - from:
    - namespaceSelector:
        matchLabels:
          app: users-backend
    ports:
    - protocol: TCP
      port: 80
```

Create the NetworkPolicy:
```
kubectl create -f np-users-backend-80.yml
```

Check the status of the exam objectives:
```
./verify.sh
```