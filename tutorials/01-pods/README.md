
# Launch nginx

```
kubectl apply -f nginx.yaml
```

Check the deployment status

```
kubectl get pods
```

Open a tunnel to visit the pod

```
kubectl port-forward nginx-demo 8080:80
```

```
firefox http://localhost:8080
```

```
kubectl delete -f nginx.yaml
```

```
kubectl apply -f nginx-v2.yaml
```


# Create a index.html and create a pod that mount the directory 

```
vagrant ssh node-1 -c "sudo mkdir -p /data/01-pod/test-pd;sudo chmod 777 /data/01-pod/test-pd;  sudo echo Hello > /data/01-pod/test-pd/index.html"
kubectl apply -f test-webserver.yaml
kubectl port-forward pod/test-pd  8080:80
firefox http://localhost:8080/
```
