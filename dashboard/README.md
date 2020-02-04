
# Kubernetes dashboard

Follow the istructions below (https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/) :

```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.0.0-beta8/aio/deploy/recommended.yaml
kubectl get pods -n kubernetes-dashboard           # wait until running
cd dashboard
kubectl apply -f 01-service-account.yaml
kubectl apply -f 02-clusterbindingrole.yaml
./get-token.sh
kubectl proxy
```

Open your navigator and go to : http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/