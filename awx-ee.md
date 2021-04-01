# define dedicate ee image for AWX templates

Create an admin account to create pods
```
cat <<-EOF | kubectl apply -f -
apiVersion: v1
kind: ServiceAccount
metadata:
  name: awx-admin
  namespace: kube-system
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: cluster-admin-rolebinding
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
- kind: ServiceAccount
  name: awx-admin
  namespace: kube-system
EOF
```

Print account token

```
kubectl -n kube-system get secret $SECRET_NAME  -o json | jq -r '.data["token"] ' | base64 -d
```

Print kubernetes certificate

```
kubectl -n kube-system get secret $SECRET_NAME  -o json | jq -r '.data["ca.crt"]' | base64 -d
```

In AWX
**create a credential (type : OpenShift or Kubernetes API Bearer Token)**

![image](https://user-images.githubusercontent.com/39823762/113328716-fc86eb80-931c-11eb-9cae-62ad9d2ff536.png)


**Create a new instance group**

![image](https://user-images.githubusercontent.com/39823762/113328999-4cfe4900-931d-11eb-8e21-ce01c320589f.png)

Select **Customize pod specification** and change container image with :
```
localhost:32000/awx-ee-f5
```

In Templates, change the instance group to this new instance group.
