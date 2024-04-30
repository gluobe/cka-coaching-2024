# Question 05 - Sorting

## Pods in all namespaces sorted by age

Use the command below:
```
kubectl get pods -A --sort-by="metadata.creationTimestamp"
```

## Pods in all namespaces sorted by uid

Use the command below:
```
kubectl get pods -A --sort-by="metadata.uid"
```