
# Launch nginx

```
kubectl apply -f nginx.yaml
```

Check the deployment status

```
kubectl get pods
```

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