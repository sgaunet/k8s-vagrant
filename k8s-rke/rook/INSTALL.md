# Rook ceph setup

Extract the sources 

```
cd rook
git clone https://github.com/rook/rook.git
cd ..
```

Create the rook opeator

```
kubectl --kubeconfig kube_config_cluster.yml apply -f rook/rook/cluster/examples/kubernetes/ceph/common.yaml
kubectl --kubeconfig kube_config_cluster.yml apply -f rook/rook/cluster/examples/kubernetes/ceph/operator.yaml
```

At this step, you will get something like this :

```
kubectl --kubeconfig kube_config_cluster.yml get po -n rook-ceph
NAME                                 READY   STATUS    RESTARTS   AGE
rook-ceph-operator-db86d47f5-gvqtp   1/1     Running   0          148m
rook-discover-29hjh                  1/1     Running   0          145m
rook-discover-7q79z                  1/1     Running   0          145m
rook-discover-hsjz2                  1/1     Running   0          145m
rook-discover-qxbhm                  1/1     Running   0          145m
rook-discover-s79ht                  1/1     Running   0          145m
```

OK, time to create the ceph cluster with cluster-ceph.yaml :

```
kubectl --kubeconfig kube_config_cluster.yml apply -f rook/cluster-ceph.yaml
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

and a persistentvolume for nginx for example :

```
kubectl --kubeconfig kube_config_cluster.yml apply -f  rook/nginx/pvc.yaml 
persistentvolumeclaim/nginx-pvc created
$ kubectl --kubeconfig kube_config_cluster.yml  get pvc
NAME        STATUS   VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS      AGE
nginx-pvc   Bound    pvc-083bd094-fb69-4567-8843-9be6e820ef77   50Mi       RWO            rook-ceph-block   21s
```

Finally, test with nginx deployment :

```
$ kubectl --kubeconfig kube_config_cluster.yml apply -f rook/nginx/nginx.yaml 
deployment.apps/nginx created
```
