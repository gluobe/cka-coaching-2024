# Count the Number of Nodes That Are Ready to Run Normal Workloads

Switch to the appropriate context with kubectl:
```
kubectl config use-context acgk8s
```

Count the number of nodes ready to run a normal workload:
```
kubectl get nodes
```

Check that the worker nodes can run normal workloads:
```
kubectl describe node acgk8s-worker1
```

Scroll to the top of the output and check the list of taints. You should see none.

Repeat the steps above for acgk8s-worker2. You should see no taints on that node either.

Save this number to the file /k8s/0001/count.txt:
```
echo 2 > /k8s/0001/count.txt
```

Run the verification script to check your work:
```
./verify.sh
```

# Retrieve Error Messages from a Container Log

Obtain error messages from a container's log:
```
kubectl logs -n backend data-handler -c proc
```

Return only the error messages:
```
kubectl logs -n backend data-handler -c proc | grep ERROR
```

Save this output to the file /k8s/0002/errors.txt:
```
kubectl logs -n backend data-handler -c proc | grep ERROR > /k8s/0002/errors.txt
```

Run the verification script to check your work:
```
./verify.sh
```

# Find the Pod with a Label of app=auth in the Web Namespace That Is Utilizing the Most CPU

> Before doing this step, please wait a minute or two to give our backend script time to generate CPU load.

Locate which Pod in the web namespace with the label app=auth is using the most CPU (in some cases, other Pods may show as consuming more CPU):
```
kubectl top pod -n web --sort-by cpu --selector app=auth
```

Save the name of this Pod to /k8s/0003/cpu-pod.txt:
```
echo auth-web > /k8s/0003/cpu-pod.txt
```

Run the verification script to check your work:
```
./verify.sh
```