## Install AWX operator

```
kubectl apply -f https://raw.githubusercontent.com/ansible/awx-operator/0.11.0/deploy/awx-operator.yaml
````
Note: This command line can change according to [awx-operator github](https://github.com/ansible/awx-operator)  
  current operator version is 0.11.0 (defined in URL above).   
  Starting with operator 0.11.0, **tower_** prefix is removed from all parameters.  

## Install AWX
[AWX declaration file](awx.yml)

*Change tower_hostname before apply*
```
---
apiVersion: awx.ansible.com/v1beta1
kind: AWX
metadata:
  name: tower
spec:
  hostname: tower.demo.local
  image: quay.io/ansible/awx
  image_version: latest
  image_pull_policy: Always
  admin_user: admin
  ingress_type: Ingress
  ingress_annotations: |
      kubernetes.io/ingress.class: public
```
[AWX secret file](secret.yml)
```
---
apiVersion: v1
kind: Secret
metadata:
  name: tower-admin-password
  namespace: default
stringData:
  password: "MySuperLongPassword"
```

Install secret from file

```
kubectl apply -f secret.yml
```

Install AWX from file

```
kubectl apply -f awx.yml
```
