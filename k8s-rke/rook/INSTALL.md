# Rook ceph setup

Extract the sources 

```
git clone https://github.com/rook/rook.git
```

Create the rook opeator

```
kubectl --kubeconfig kube_config_cluster.yml apply -f rook/rook/cluster/examples/kubernetes/ceph/common.yaml
kubectl --kubeconfig kube_config_cluster.yml apply -f rook/rook/cluster/examples/kubernetes/ceph/operator.yaml
```

At this step, you will get something like this :



OK, time to create the ceph cluster with cluster-ceph.yaml :

```
kubectl apply -f cluster-ceph.yaml
```

Now you can also run the toolbox :

```
kubectl --kubeconfig kube_config_cluster.yml apply -f rook/rook/cluster/examples/kubernetes/ceph/toolbox.yaml
```

With the toolbox, you can check the cluster :

```
```

Create the storageclass :

```
kubectl --kubeconfig kube_config_cluster.yml apply -f rook/rook/cluster/examples/kubernetes/ceph/csi/rbd/storageclass.yaml
```

and the persistentvolume :

```
kubectl --kubeconfig kube_config_cluster.yml apply -f rook/pvc.yaml
```

Finally, test with mongo :

```
kubectl --kubeconfig kube_config_cluster.yml apply -f rook/
```
