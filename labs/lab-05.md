# Lab 05 - kubectl

## Documentation

* https://kubernetes.io/docs/reference/kubectl/ (search: kubectl)

## Task 1 - get PV volumes sorted by capacity and store it in a file

```
kubectl get pv
```

```
kubectl get pv -o yaml
```

```
kubectl get pv --sort-by=.spec.capacity.storage > pv.txt
```

## Task 2 - get the key from inside a pod and store it in a file

```
kubectl exec quark -n beebox-mobile -- cat /etc/key/key.txt > key.txt
```

## Task 3 - kubectl apply

```
kubectl apply -f deployment.yaml
```

```
kubectl delete -f deployment.yaml
```

```
kubectl apply -f folder/
```

## Task 4 - delete service in beebox-mobile namespace

```
kubectl get svc -n beebox-mobile
```

```
kubectl delete svc beebox-auth-svc -n beebox-mobile
```