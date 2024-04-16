# Lab 09 - Self-healing containers

## Documenation

* https://kubernetes.io/docs/concepts/workloads/pods/pod-lifecycle/ (search: livenessprobe or onfailure)
* https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/ (search: livenessprobe task)

## Task 1 - Set a Restart Policy to Restart the Container When It Is Down
Find the pod that needs to be modified:
```
kubectl get pods -o wide
```

Take note of the beebox-shipping-data pod's IP address.

Use the busybox pod to make a request to the pod to see if it is working:
```
kubectl exec busybox -- curl <beebox-shipping-data_ IP>:8080
```

We will likely get an error message.

Edit the pod's YAML descriptor:
```
kubectl edit pod beebox-shipping-data
```

Set the restartPolicy to Always:
```
spec:
  ...
  restartPolicy: Always
  ...
```

## Task 2 - Create a Liveness Probe to Detect When the Application Has Crashed

Add a liveness probe:
```
spec:
  containers:
  - ...
    name: shipping-data
    livenessProbe:
      httpGet:
        path: /
        port: 8080
      initialDelaySeconds: 5
      periodSeconds: 5
    ...
```

Delete the pod:
```
kubectl delete pod beebox-shipping-data
```

Re-create the pod to apply the changes:
```
kubectl apply -f beebox-shipping-data.yml
```

Check the pod status
```
kubectl get pods -o wide
```

If you wait a minute or so and check again, you should see the pod is being restarted whenever the application crashes.

Check the http response from the pod again (it will have a new IP address since we re-created it):
```
kubectl exec busybox -- curl <beebox-shipping-data_IP>:8080
```

If you wish, you can explore and see what happens as the application crashes and the pod is restarted automatically.

> NOTE: you cannot use `kubectl edit pod` here as it will result in an error (you cannot make changes to certain fields of a running pod):
```
# Please edit the object below. Lines beginning with a '#' will be ignored,
# and an empty file will abort the edit. If an error occurs while saving this file will be
# reopened with the relevant failures.
#
# pods "beebox-shipping-data" was not valid:
# * spec: Forbidden: pod updates may not change fields other than `spec.containers[*].image`,`spec.initContainers[*].image`,`spec.activeDeadlineSeconds`,`spec.tolerations` (only additions to existing tolerations),`spec.terminationGracePeriodSeconds` (allow it to be set to 1 if it was previously negative)
```