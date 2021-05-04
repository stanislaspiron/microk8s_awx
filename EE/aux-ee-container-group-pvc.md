# Enable persistent volume for AWX-EE container
AWX-EE is defined to create a dedicated container for every job. some playbooks requires to share data between jobs, or store files to upload to job targets.
This configuration allow to add /var/tmp in persistent volume.

As this repository is focused to configure AWX in microk8s cluster, we will use the default storage class defined to use local disk.

Create a persistent volume claim

```
kubectl apply -f awx-ee-pvc.yml
```

**Create a new instance group or update an existing instance group**

![image](https://user-images.githubusercontent.com/39823762/113328999-4cfe4900-931d-11eb-8e21-ce01c320589f.png)

Select **Customize pod specification** and change container image with :

```
apiVersion: v1
kind: Pod
metadata:
  namespace: default
spec:
  volumes:
  - name: awx-ee-tmp
    persistentVolumeClaim:
      claimName: awx-ee
  containers:
    - image: 'quay.io/ansible/awx-ee:0.1.1'
      name: worker
      volumeMounts:
      - name: awx-ee-tmp
        mountPath: /var/tmp/
        subPath: tmp
```

In Templates, change the instance group to this new instance group.
