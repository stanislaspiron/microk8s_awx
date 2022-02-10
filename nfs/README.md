# Configure Dynamic NFS storage Class

## Install nfs-common package on kubernetes server(s)

```
sudo apt install nfs-common
```

## Deploy NFS deployment


**Create RBAC roles to configure persistent volumes**

```
kubectl apply -f rbac.yml
```


**Create Deployment for NFS provisioner**

Before applying this deployment, change the NFS server IP address and path.

```
kubectl apply -f deployment.yml
```

**Create Storage Class**

```
kubectl apply -f storageclass.yml
```

Source : 
- https://medium.com/@myte/kubernetes-nfs-and-dynamic-nfs-provisioning-97e2afb8b4a9
- https://tuananh.org/2020/05/01/using-synology-nfs-as-external-storage-with-k8s/
