# Microk8s NFS storage

source: https://flores.eken.nl/running-unifi-controller-on-k8s/

## Create Persistent Volume
```
---
apiVersion: v1
kind: PersistentVolume
metadata:
  name: synology-nfs
spec:
  storageClassName: "nfs"
  capacity:
    storage: 100Gi
  accessModes:
    - ReadWriteMany
  nfs:
    server: 192.168.1.249
    path: "/volume1/kube-data"
```

## Create shared Persistent Volume Claim
```
---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: nfs-shared
spec:
  storageClassName: "nfs"
  accessModes:
    - ReadWriteMany
  resources:
    requests:
      storage: 100Gi
```