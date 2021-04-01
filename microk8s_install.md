# Install microk8s
Source : https://microk8s.io/docs

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
