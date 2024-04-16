# Lab 14 - Deployments

## Documenation

* https://kubernetes.io/docs/concepts/workloads/controllers/deployment/ (search: deployments)

## Task 1 - Update the App to a New Version of the Code

Edit the beebox-web deployment:
```
kubectl edit deployment beebox-web
```

Locate the Pod's container specification, and change the 1.0.1 image version tag to 1.0.2:
```
...

spec:
  containers:
  - image: acgorg/beebox-web:1.0.2
    imagePullPolicy: IfNotPresent
    name: web-server

...
```

Check the status of your deployment to watch the rolling update occur:
```
kubectl rollout status deployment.v1.apps/beebox-web
```

## Task 2 - Scale the App to a Larger Number of Replicas

Scale the deployment to 5 replicas:
```
kubectl scale deployment.v1.apps/beebox-web --replicas=5
```

View the deployment to watch it scale up:
```
kubectl get deployment beebox-web
```

View the Pods:
```
kubectl get pods
```