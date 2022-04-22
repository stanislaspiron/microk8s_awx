## Install AWX operator and AWX

## Description
This replository contains Yaml Files to deploy AWX in Kubernetes cluster.
This repository uses AWX Operator to manage deployment.

## Requirements
This deployment requires following configuration:
- kubernetes cluster. This was tested in [Microk8s](https://github.com/stanislaspiron/microk8s_awx/blob/main/microk8s/microk8s_install.md)
- nfs [storage class](https://github.com/stanislaspiron/microk8s_awx/blob/main/nfs/README.md)

## Install 
Install kustomize package. 

```
curl -s "https://raw.githubusercontent.com/kubernetes-sigs/kustomize/master/hack/install_kustomize.sh" | bash 
```
***Note: when I updated this documentation, kustomize from snap was not working.***

## Create the deployment

This uses following parameters:
- AWX and related objects will be installed in **awx** namespace
- AWX deployment will be named : **tower**
- Ingress type is : **Ingress**
- Ingress FQDN is : **tower.demo.local**
- postgresql database is deployed with AWX
- Postgresql data is stored on external NFS storage
- AWX TLS secret uses awx-tls-secret.yml. for obvious reason, I did not published my own certificate and key in the public file. if you don't want to create your own certificate, comment following lines:
  - ***- awx-tls-secret.yml*** in kustomization.yaml file
  - ***ingress_tls_secret: tower-secret-tls*** in awx.yml file

**kustomization.yaml**

```
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization
resources:
  # Find the latest tag here: https://github.com/ansible/awx-operator/releases
  - github.com/ansible/awx-operator/config/default?ref=0.20.0
  - awx-tls-secret.yml
  - secret.yml
  - awx.yml

# Set the image tags to match the git version from above
images:
  - name: quay.io/ansible/awx-operator
    newTag: 0.20.0

# Specify a custom namespace in which to install AWX
namespace: awx
```

Run following command:

```
/usr/bin/kustomize build . | kubectl apply -f -
```
