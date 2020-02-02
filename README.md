# k8s-vagrant

Create a kubernetes cluster (based on informations in https://kubernetes.io/blog/2019/03/15/kubernetes-setup-using-ansible-and-vagrant/ but with modifications to be functionnal ...)

Need :

* vagrant
* virtualbox
* a PC with at least 8 Go (16 would be better and more if you want a big cluster (update vagrantfile))


# Go on

## Create the k8s cluster 

```
vagrant up
```

## Configure kubectl on your laptop

### Install kubectl

[Follow this guide](https://kubernetes.io/fr/docs/tasks/tools/install-kubectl/#installer-kubectl-sur-linux)

### Configuration

```
mkdir ~/.kube
vagrant ssh k8s-master -c "sudo cat /etc/kubernetes/admin.conf" > ~/.kube/config
```

## Check 

* List the nodes :

```
kubectl get nodes
```

* Versions of APIs

```
 kubectl api-versions
```

* Version of kubectl and cluster

```
kubectl version
```

# Documentation

[kubernetes documentation](https://kubernetes.io/docs/concepts/overview/what-is-kubernetes/)

In tutorials, there is some examples, some commands but actually, no comment. That's just memo for me. Maybe I will complete this later... don't know (depending time consuming)

