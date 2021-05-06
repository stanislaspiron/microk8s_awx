# Configure Dynamic NFS storage Class

Source : https://medium.com/@myte/kubernetes-nfs-and-dynamic-nfs-provisioning-97e2afb8b4a9

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
