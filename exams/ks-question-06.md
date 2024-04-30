# Question 06 - Persistent Storage

## Create persistent volume

Use the manifest below:
```
apiVersion: v1
kind: PersistenVolume
metadata:
  name: safari-pv
spec:
  capacity:
    storage: 2Gi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: "/Volumes/Data"
```

## Create persistent volume claim

Use the manifest below:
```
apiVersion: v1
kind: PersistenVolumeClaim
metadata:
  name: safari-pvc
  namespace: project-tiger
spec:
  capacity:
    storage: 2Gi
  accessModes:
    - ReadWriteOnce
```

## Create deployment

Use the manifest below:
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: safari
  namespace: project-tiger
  labels:
    app: httpd
spec:
  replicas: 3
  selector:
    matchLabels:
      app: httpd
  template:
    metadata:
      labels:
        app: httpd
    spec:
      containers:
      - name: httpd
        image: httpd:2.4.41-alpine
        volumeMounts:
        - name: vol
          mountPath: /tmp/safari-data   
      volumes:
      - name: vol
        persistentVolumeClaim:
          claimName: safari-pvc     
```