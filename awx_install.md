## Install AWX operator
```
kubectl apply -f https://raw.githubusercontent.com/ansible/awx-operator/devel/deploy/awx-operator.yaml
````

## Install AWX
AWX declaration file (awx.yaml)
```
---
apiVersion: awx.ansible.com/v1beta1
kind: AWX
metadata:
  name: tower
spec:
  tower_hostname: tower.demo.local <<< Change this hostname
  tower_admin_user: admin
  tower_ingress_type: Ingress
  tower_ingress_annotations: |
      kubernetes.io/ingress.class: public
---
apiVersion: v1
kind: Secret
metadata:
  name: tower-admin-password
  namespace: default
stringData:
  password: "MySuperLongPassword"
```

Install AWX from file

```
kubectl apply -f awx.yaml
```
