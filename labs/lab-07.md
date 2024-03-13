# Lab 07 - Pod Resource Usage

## Documenation

* https://kubernetes.io/docs/tasks/debug/debug-cluster/resource-metrics-pipeline/#metrics-server (search: metrics-server)

## Task 1 - Install metrics-server

```
kubectl apply -f https://raw.githubusercontent.com/ACloudGuru-Resources/content-cka-resources/master/metrics-server-components.yaml
```

## Task 2 - Locate CPU-hogging Pod

```
kubectl top pod -n beebox-mobile --sort-by cpu --selector app=auth
```