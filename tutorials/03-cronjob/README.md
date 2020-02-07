

```
kubectl apply -f cronjob.yaml
```


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


# Persistance

vagrant ssh node-1 -c "sudo mkdir -p /data/03-cronjob/;sudo chmod 777 /data/03-cronjob"

```
kubectl apply -f cronjob-v2.yaml
```

 kubectl get cronjob                            
NAME    SCHEDULE      SUSPEND   ACTIVE   LAST SCHEDULE   AGE
hello   */1 * * * *   False     0        36s             77s


vagrant ssh node-1 -c "cat  /data/03-cronjob/mycronjob.txt"

Hello from the Kubernetes cluster 2020-02-07 20:36
