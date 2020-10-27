# RKE

Rancher Kubernetes Engine (RKE) is a CNCF-certified Kubernetes distribution that runs entirely within Docker containers. It works on bare-metal and virtualized servers. RKE solves the problem of installation complexity, a common issue in the Kubernetes community. With RKE, the installation and operation of Kubernetes is both simplified and easily automated, and it’s entirely independent of the operating system and platform you’re running. As long as you can run a supported version of Docker, you can deploy and run Kubernetes with RKE.

[Download rke and install it](https://rancher.com/docs/rke/latest/en/installation/#download-the-rke-binary)

# Configure

In the k8s-rke folder, you have a setup to create a clsuter of 5 nodes :

* 3 master
* 2 workers

Two files are important :

* vagrantfile : used by vagrant to create the VM
* cluster.yml : used by rke to initialise, launch the k8s cluster

If you want to modify the number of nodes, edit those files, very easy.

Actually, the VMs are 

| VM     | Type   | IP         |
| ------ | ------ | ---------- |
| rke-0  | master | 10.0.60.10 |
| rke-1  | master | 10.0.60.11 |
| rke-2  | master | 10.0.60.12 |
| rke-3  | worker | 10.0.60.13 |
| rke-4  | worker | 10.0.60.14 |


Here is a cluster which will recommend a powerful laptop (2Go RAM by VM and 2 vcpu).
Be carefull, there is also an axtra disk (/dev/sdb) by VM. Every disk is created under the data folder (data/disk-XX.vdi). In the vagranfile, this disk is fixed to 50 Gb. Reduce it you don't have enough spaces.

Why adding such a disk ? Because after creating the cluster, you will be able to install rook/ceph and it needs some spaces to allow to play with Persistent Volumes. (Will be added soon)


# Create cluster

First, create the VM

```
vagrant up
```

The created VM are ubuntu 18.04 with prerequisites.

Create kubernetes

```
rke up
```

rke will create two files :
* kube_config_cluster.yml : which is the kubeconfig file
* cluster.rkestate : State of the cluster, do not modify it, this is for rke


Check and list the nodes :

```
kubectl --kubeconfig kube_config_cluster.yml get nodes
```

# Install rook/ceph operator :

Stay in this folder and follow the rook/INSTALL.md guide.

