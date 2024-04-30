# Question 03 - Scale the amount of pods for a specific workload

To find out which workload type the pods are part of issue the following command:
```
kubectl describe pod o3db-0 -n project-c13 | grep -i controlled
```

You will get:
```
Controlled By: StatefulSet/o3db
```

We now know it is a StatefulSet workload type, so we can scale it as follows:
```
kubectl scale statefulset o3db -n project-c13 --replicas=1
```