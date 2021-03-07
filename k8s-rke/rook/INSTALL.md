# Rook ceph setup

I'm not an expert of rook, please, read the quickstart and the rook/Ceph documentation quickstart. The documentation below is probably not up to date... I will probably not update it, do not have time for this. 


Extract the sources 

```
cd rook
git clone -b v1.5.8  https://github.com/rook/rook.git
cd ..
```

Create the rook opeator

```
kubectl --kubeconfig kube_config_cluster.yml apply -f rook/rook/cluster/examples/kubernetes/ceph/common.yaml
kubectl --kubeconfig kube_config_cluster.yml apply -f rook/rook/cluster/examples/kubernetes/ceph/crds.yaml
kubectl --kubeconfig kube_config_cluster.yml apply -f rook/rook/cluster/examples/kubernetes/ceph/operator.yaml
```

At this step, you will get something like this :

```
kubectl --kubeconfig kube_config_cluster.yml get po -n rook-ceph
NAME                                 READY   STATUS    RESTARTS   AGE
rook-ceph-operator-db86d47f5-gvqtp   1/1     Running   0          148m
```

OK, time to create the ceph cluster with cluster-ceph.yaml :

```
kubectl --kubeconfig kube_config_cluster.yml apply -f rook/cluster-ceph.yaml
```

Now you can also run the toolbox :

```
kubectl --kubeconfig kube_config_cluster.yml apply -f rook/rook/cluster/examples/kubernetes/ceph/toolbox.yaml
```

Now there are some pods in the rook-ceph namespace :

```
kubectl --kubeconfig kube_config_cluster.yml get po -n rook-ceph
NAME                                                   READY   STATUS      RESTARTS   AGE
csi-cephfsplugin-dtgbd                                 3/3     Running     3          21h
csi-cephfsplugin-ljgfv                                 3/3     Running     3          21h
csi-cephfsplugin-p7lcg                                 3/3     Running     3          21h
csi-cephfsplugin-provisioner-c68f789b8-l5kdf           6/6     Running     6          21h
csi-cephfsplugin-provisioner-c68f789b8-vdn7x           6/6     Running     6          21h
csi-cephfsplugin-w8s5t                                 3/3     Running     3          21h
csi-cephfsplugin-wwbvn                                 3/3     Running     3          21h
csi-rbdplugin-2ssnt                                    3/3     Running     3          21h
csi-rbdplugin-glgbf                                    3/3     Running     3          21h
csi-rbdplugin-mmvns                                    3/3     Running     3          21h
csi-rbdplugin-njlx5                                    3/3     Running     3          21h
csi-rbdplugin-nlzcg                                    3/3     Running     3          21h
csi-rbdplugin-provisioner-5bc97cdbb9-8rs7v             6/6     Running     6          21h
csi-rbdplugin-provisioner-5bc97cdbb9-hp7rk             6/6     Running     6          21h
rook-ceph-crashcollector-10.0.60.10-7d65888bdc-67ppk   1/1     Running     1          20h
rook-ceph-crashcollector-10.0.60.11-6f7d8fb6b-d2pgl    1/1     Running     1          20h
rook-ceph-crashcollector-10.0.60.12-7cbb8fdfb9-qb4hb   1/1     Running     1          20h
rook-ceph-crashcollector-10.0.60.13-5cbf94db5b-58fvw   1/1     Running     1          20h
rook-ceph-crashcollector-10.0.60.14-564bd7d855-gt4gb   1/1     Running     1          20h
rook-ceph-mgr-a-54b9df89c9-x9plr                       1/1     Running     1          20h
rook-ceph-mon-a-694954848-swvwd                        1/1     Running     1          21h
rook-ceph-mon-b-579b887f77-nqk2l                       1/1     Running     1          20h
rook-ceph-mon-c-54c9856fdb-q957c                       1/1     Running     1          20h
rook-ceph-operator-5865fcf468-gl4fh                    1/1     Running     1          21h
rook-ceph-osd-0-754c66466c-xn6wt                       1/1     Running     1          20h
rook-ceph-osd-1-678c6b8b7-j5x7l                        1/1     Running     1          20h
rook-ceph-osd-2-7d686b59c9-nbpc6                       1/1     Running     1          20h
rook-ceph-osd-3-6b69f49cdc-jsbfs                       1/1     Running     1          20h
rook-ceph-osd-4-9774fccc9-qzsz7                        1/1     Running     2          20h
rook-ceph-osd-prepare-10.0.60.10-ttk9m                 0/1     Completed   0          20m
rook-ceph-osd-prepare-10.0.60.11-h6wbf                 0/1     Completed   0          20m
rook-ceph-osd-prepare-10.0.60.12-ddkwm                 0/1     Completed   0          20m
rook-ceph-osd-prepare-10.0.60.13-wtmsh                 0/1     Completed   0          20m
rook-ceph-osd-prepare-10.0.60.14-86mjq                 0/1     Completed   0          20h
rook-ceph-tools-585d9b6954-248jb                       1/1     Running     0          29s
```

With the toolbox, you can check the cluster :

```
kubectl --kubeconfig kube_config_cluster.yml -n rook-ceph get pod -l "app=rook-ceph-tools"
NAME                               READY   STATUS    RESTARTS   AGE
rook-ceph-tools-585d9b6954-248jb   1/1     Running   0          5m11s
```

```
kubectl --kubeconfig kube_config_cluster.yml  -n rook-ceph exec -it rook-ceph-tools-585d9b6954-248jb -- ceph status
  cluster:
    id:     7cc21229-74dd-41a9-98cb-f041c82d6a6e
    health: HEALTH_OK
 
  services:
    mon: 3 daemons, quorum b,a,c (age 26m)
    mgr: a(active, since 25m)
    osd: 5 osds: 5 up (since 25m), 5 in (since 20h)
 
  data:
    pools:   0 pools, 0 pgs
    objects: 0 objects, 0 B
    usage:   5.0 GiB used, 245 GiB / 250 GiB avail
    pgs:     

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


```
kubectl --kubeconfig kube_config_cluster.yml get po
NAME                     READY   STATUS    RESTARTS   AGE
nginx-5f4c55cb9d-2l29n   1/1     Running   0          65s
```

In a VM, after the setup of rook ceph, you can see the sdb partition handled by ceph.

```
vagrant@rke-0:~$ lsblk
NAME                                                                                                  MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda                                                                                                     8:0    0   64G  0 disk 
└─sda1                                                                                                  8:1    0   64G  0 part 
  ├─vagrant--vg-root                                                                                  253:1    0   63G  0 lvm  /
  └─vagrant--vg-swap_1                                                                                253:2    0  980M  0 lvm  
sdb                                                                                                     8:16   0   50G  0 disk 
└─ceph--98d74f4e--97de--4013--bee1--a6b8625a9310-osd--block--d3e7e43a--381e--47c2--a4f3--42bf148e2c7d 253:0    0   50G  0 lvm  
```

Happy kubernetes !