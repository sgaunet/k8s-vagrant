
```
kubectl get cronjob
```

```
kubectl get pods
NAME                     READY   STATUS      RESTARTS   AGE
hello-1580766720-m9n49   0/1     Completed   0          3m36s
hello-1580766780-z7m44   0/1     Completed   0          2m36s
hello-1580766840-2glg9   0/1     Completed   0          95s
hello-1580766900-nvg77   0/1     Completed   0          35s
```

```
$ kubectl  logs hello-1580766840-2glg9 
Mon Feb  3 21:54:12 UTC 2020
Hello from the Kubernetes cluster
```