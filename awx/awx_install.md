## Install AWX operator

```
kubectl apply -f https://raw.githubusercontent.com/ansible/awx-operator/0.9.0/deploy/awx-operator.yaml
````
Note: This command line can change according to [awx-operator github](https://github.com/ansible/awx-operator)
  current operator version is 0.9.0 (defined in URL above)

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
  tower_hostname: tower.demo.local
  tower_admin_user: admin
  tower_image: quay.io/ansible/awx
  tower_image_version: 19.1.0
  tower_image_pull_policy: Always
  tower_ingress_type: Ingress
  tower_ingress_annotations: |
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
