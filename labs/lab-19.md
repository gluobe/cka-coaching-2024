# Lab 19 - Volumes

## Documenation

* https://kubernetes.io/docs/tasks/configure-pod-container/configure-volume-storage/ (search: volume task)

## Task 1 - Create a Pod That Outputs Data to the Host Using a Volume

Create a Pod that will interact with the host file system:
```
vi maintenance-pod.yml
```

Enter in the first part of the basic YAML for a simple busybox Pod that outputs some data every five seconds to the host's disk:
```
apiVersion: v1
kind: Pod
metadata:
    name: maintenance-pod
spec:
    containers:
    - name: busybox
      image: busybox
      command: ['sh', '-c', 'while true; do echo Success! >> /output/output.txt; sleep 5; done']
```

Under the basic YAML, begin creating volumes, which should be level with the containers spec:
```
volumes:
- name: output-vol
  hostPath:
      path: /var/data
```

In the containers spec of the basic YAML, add a line for volume mounts:
```
volumeMounts:
- name: output-vol
  mountPath: /output
```

Check that the final maintenance-pod.yml file looks like this:
```
apiVersion: v1
kind: Pod
metadata:
    name: maintenance-pod
spec:
    containers:
    - name: busybox
      image: busybox
      command: ['sh', '-c', 'while true; do echo Success! >> /output/output.txt; sleep 5; done']

      volumeMounts:
      - name: output-vol
        mountPath: /output

    volumes:
    - name: output-vol
      hostPath:
        path: /var/data
```

Finish creating the Pod:
```
kubectl create -f maintenance-pod.yml
```

Make sure the Pod is up and running:
```
kubectl get pods
```

Check that maintenance-pod is running, so it should be outputting data to the host system.

Log into the worker node server using the credentials provided:
```
ssh cloud_user@<PUBLIC_IP_ADDRESS>
```

Look at the output to see whether the Pod setup was successful:
```
cat /var/data/output.txt
```

## Task 2 - Create a Multi-Container Pod That Shares Data Between Containers Using a Volume

Return to the control plane server.

Create another YAML file for a shared-data multi-container Pod:
```
vi shared-data-pod.yml
```

Start with the basic Pod definition and add multiple containers, where the first container will write the output.txt file and the second container will read the output.txt file:
```
apiVersion: v1
kind: Pod
metadata:
    name: shared-data-pod
spec:
    containers:
    - name: busybox1
      image: busybox
      command: ['sh', '-c', 'while true; do echo Success! >> /output/output.txt; sleep 5; done']
    - name: busybox2
      image: busybox
      command: ['sh', '-c', 'while true; do cat /input/output.txt; sleep 5; done']
```

Set up the volumes, again at the same level as containers with an emptyDir volume that only exists to share data between two containers in a simple way:
```
volumes:
- name: shared-vol
  emptyDir: {}
```

Mount that volume between the two containers by adding the following lines under command for the busybox1 container:
```
volumeMounts:
- name: shared-vol
  mountPath: /output
```

For the busybox2 container, add the following lines to mount the same volume under command to complete creating the shared file:
```
volumeMounts:
- name: shared-vol
  mountPath: /input
```

Make sure that the final shared-data-pod.yml file looks like this:
```
apiVersion: v1
kind: Pod
metadata:
    name: shared-data-pod
spec:
    containers:
    - name: busybox1
      image: busybox
      command: ['sh', '-c', 'while true; do echo Success! >> /output/output.txt; sleep 5; done']
      volumeMounts:
      - name: shared-vol
        mountPath: /output
    - name: busybox2
      image: busybox
      command: ['sh', '-c', 'while true; do cat /input/output.txt; sleep 5; done']
      volumeMounts:
      - name: shared-vol
        mountPath: /input
    volumes:
    - name: shared-vol
      emptyDir: {}  
```

Finish creating the multi-container Pod:
```
kubectl create -f shared-data-pod.yml
```

Make sure the Pod is up and running and check if both containers are running and ready:
```
kubectl get pods
```

To make sure the Pod is working, check the logs for shared-data-pod.yml and specify the second container that is reading the data and printing it to the console:
```
kubectl logs shared-data-pod -c busybox2
```

If you see the series of Success! messages, you have successfully created both containers, one of which is using a host path volume to write some data to the host disk and the other of which is using an emptyDir volume to share a volume between two containers in the same Pod.