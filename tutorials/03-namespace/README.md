

kubectl apply -f dev.json


kubectl get namespaces --show-labels


kubectl apply -f nginx.yaml
kubectl get pods --all-namespaces
kubectl get pods --namespace=development
