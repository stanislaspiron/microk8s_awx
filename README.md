# How to deploy AWX in microk8s

## Install microk8s
https://ubuntu.com/kubernetes/install#single-node


```
sudo snap install microk8s --classic
```

Add required user rights to execute microk8s commands.

```
sudo usermod -a -G microk8s ${USER}
sudo chown -f -R ${USER} ~/.kube
```
logout and log in again

Add alias for kubectl
```
sudo snap alias microk8s.kubectl kubectl
```

Install microk8s addons
- **dns** to support communication between pods (postgre is a dedicated pod)
- **storage** to support persistent volume for database persistence
- **ingress** to install nginx ingress controller to publish services outside kubernetes
- **dashboard** to display the kubernetes dashboard (optional)
```
microk8s enable dns storage ingress dashboard
````

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

## Optional : configure dashboard ingress
dashboard ingress declaration (dashboard-ingress.yaml)
```
---
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  annotations:
    kubernetes.io/ingress.class: public
    nginx.ingress.kubernetes.io/backend-protocol: "HTTPS"
  name: dashboard
  namespace: kube-system
spec:
  rules:
  - host: microk8s.demo.local <<< Change this hostname
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: kubernetes-dashboard
            port:
              number: 443
```

Install dashboard ingress from file

```
kubectl apply -f dashboard-ingress.yaml
```

get token to authenticate to dashboard

```
token=$(microk8s.kubectl -n kube-system get secret | grep default-token | cut -d " " -f1)
microk8s.kubectl -n kube-system describe secret $token
```

