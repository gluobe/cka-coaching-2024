# Lab 20 - Persistent Volumes

## Documenation

* https://kubernetes.io/docs/tasks/configure-pod-container/configure-persistent-volume-storage/ (search: persistent volume task)

## Task 1 - Create a PersistentVolume That Allows Claim Expansion

Create a custom Storage Class by using:
```
vi localdisk.yml
```

Define the Storage Class by using:
```
apiVersion: storage.k8s.io/v1 
kind: StorageClass 
metadata: 
  name: localdisk 
provisioner: kubernetes.io/no-provisioner
allowVolumeExpansion: true
```

Finish creating the Storage Class by using:
```
kubectl create -f localdisk.yml
```

Create the PersistentVolume by using:
```
vi host-pv.yml
```

Define the PersistentVolume with a size of 1Gi by using:
```
kind: PersistentVolume 
apiVersion: v1 
metadata: 
   name: host-pv 
spec: 
   storageClassName: localdisk
   persistentVolumeReclaimPolicy: Recycle 
   capacity: 
      storage: 1Gi 
   accessModes: 
      - ReadWriteOnce 
   hostPath: 
      path: /var/output
```

Finish creating the PersistentVolume by using:
```
kubectl create -f host-pv.yml
```

Check the status of the PersistenVolume by using:
```
kubectl get pv
```

## Task 2 - Create a PersistentVolumeClaim

Start creating a PersistentVolumeClaim for the PersistentVolume to bind to by using:
```
vi host-pvc.yml
```

Define the PersistentVolumeClaim with a size of 100Mi by using:
```
apiVersion: v1 
kind: PersistentVolumeClaim 
metadata: 
   name: host-pvc 
spec: 
   storageClassName: localdisk 
   accessModes: 
      - ReadWriteOnce 
   resources: 
      requests: 
         storage: 100Mi
```

Finish creating the PersistentVolumeClaim by using:
```
kubectl create -f host-pvc.yml
```

Check the status of the PersistentVolume and PersistentVolumeClaim to verify that they have been bound:
```
kubectl get pv
kubectl get pvc
```

## Task 3 - Create a Pod That Uses a PersistentVolume for Storage

Create a Pod that uses the PersistentVolumeClaim by using:
```
vi pv-pod.yml
```

Define the Pod by using:
```
apiVersion: v1 
kind: Pod 
metadata: 
   name: pv-pod 
spec: 
   containers: 
      - name: busybox 
        image: busybox 
        command: ['sh', '-c', 'while true; do echo Success! > /output/success.txt; sleep 5; done'] 
```

Mount the PersistentVolume to the /output location by adding the following, which should be level with the containers spec in terms of indentation:
```
volumes: 
  - name: pv-storage 
    persistentVolumeClaim: 
       claimName: host-pvc
```

In the containers spec, below the command, set the list of volume mounts by using:
```
volumeMounts: 
  - name: pv-storage 
    mountPath: /output
```

Finish creating the Pod by using:
```
kubectl create -f pv-pod.yml
```

Check that the Pod is up and running by using:
```
kubectl get pods
```

If you wish, you can log in to the worker node and verify the output data by using:
```
cat /var/output/success.txt
```