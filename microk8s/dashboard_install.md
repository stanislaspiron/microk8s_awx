
## Configure dashboard ingress
[dashboard ingress declaration](dashboard-ingress.yml)

*Change hostname before apply*
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
  - host: microk8s.demo.local
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
kubectl apply -f dashboard-ingress.yml
```

get token to authenticate to dashboard

```
token=$(microk8s.kubectl -n kube-system get secret | grep default-token | cut -d " " -f1)
microk8s.kubectl -n kube-system describe secret $token
```
