# Question 04 - Liveness & Readiness probes

## Create first pod

Use a pod manifest file with the following content:
```
apiVersion: v1
kind: Pod
metadata:
  name: ready-if-service-ready
  namespace: default
spec:
  containers:
  - image: nginx:1.16.1-alpine
    name: ready-if-service-ready
    livenessProbe:
      exec:
        command:
        - 'true'
    readinessProbe:
      exec:
        command:
        - 'sh'
        - '-c'
        - 'wget -T2 -O- http://service-am-i-ready:80
```

## Create second pod

Use a pod manifest file with the following content:
```
apiVersion: v1
kind: Pod
metadata:
  name: am-i-ready
  namespace: default
spec:
  containers:
  - image: nginx:1.16.1-alpine
    name: am-i-ready
```