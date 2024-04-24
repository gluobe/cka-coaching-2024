# Lab 22 - Troubleshooting a Kubernetes application

## Documenation

* https://kubernetes.io/docs/tasks/debug/debug-cluster/ (search: journalctl)

## Task 1 - Identify What is Wrong with the Application

Examine the web-consumer deployment, which resides in the web namespace, and its Pods by using:
```
kubectl get deployment -n web web-consumer
```

Get more information, including container specifications and labels that the Pods are using, by using:
```
kubectl describe deployment -n web web-consumer
```

Look more closely at the Pods to see if both Pods are up and running by using:
```
kubectl get pods -n web
```

Get more information about the Pods and evaluate any warning messages that may come up by using:
```
kubectl describe pod -n web <POD_NAME>
```

Look at the logs associated with the container busybox by using:
```
kubectl logs -n web <POD_NAME> -c busybox
```

Determine what may be going wrong by reading the output from the container logs.

Take a closer look at the pod itself to get the data in the yaml format by using:
```
kubectl get pod -n web <POD_NAME> -o yaml
```

Determine which command is causing the errors (in this case, the `while true; do curl auth-db; sleep 5; done command`).

## Task 2 - Fix the Problem

Take a closer look at the service by using:
```
kubectl get svc -n web auth-db
```

Locate where the service is and finding the one other non-default namespace called data:
```
kubectl get namespaces
```

Check and find the auth-db service in this namespace, rather than the web namespace:
```
kubectl get svc -n data
```

Start resolving the issue by using:
```
kubectl edit deployment -n web web-consumer
```

In the spec section, scroll down to find the pod template and locate the `while true; do curl auth-db; sleep 5; done` command.

Change the command to `while true; do curl auth-db.data.svc.cluster.local; sleep 5; done` to give the fully qualified domain name of that service. This will allow the web-consumer deployment's Pods to communicate with the service successfully.

Check to ensure that the old pods have terminated and the new pods are running successfully:
```
kubectl get pods -n web
```

Check the log of one of the new pods, this time the pod should be able to communicate successfully with the service:
```
kubectl logs -n web <POD-NAME> -c busybox
```