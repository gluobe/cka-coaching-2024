# Question 13 - Multi Container Pod

Use the following manifest:
```
apiVersion: v1
kind: Pod
metadata:
  name: multi-container-playground
  namespace: default
spec:
  containers:
  - name: c1
    image: nginx:1.17.6-alpine
    env:
      - name: MY_NODE_NAME
        valueFrom:
          fieldRef:
            fieldPath: spec.nodeName
    volumeMounts:
    - name: shared-disk
      mountPath: /your/vol/path
  - name: c2
    image: busybox:1.31.1
    command:
    - 'sh'
    - '-c'
    - 'while true; do date >> /your/vol/path/date.log; sleep 1; done'
    volumeMounts:
    - name: shared-disk
      mountPath: /your/vol/path
  - name: c3
    image: busybox:1.31.1
    command:
    - 'sh'
    - '-c'
    - 'tail -f /your/vol/path/date.log'
    volumeMounts:
    - name: shared-disk
      mountPath: /your/vol/path
  volumes:
  - name: shared-disk
    emptyDir: {}
```

> NOTE: search for `MY_NODE_NAME` in docs