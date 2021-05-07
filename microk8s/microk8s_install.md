# Install microk8s
Source : https://microk8s.io/docs
## Install microk8s core
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

## Install microk8s addons
- **dns** to support communication between pods (postgre is a dedicated pod)
```
microk8s enable dns:192.168.1.1
```
- **storage** to support local storage for database persistent volume
```
microk8s enable storage
```
- **ingress** to install nginx ingress controller to publish services outside kubernetes
```
microk8s enable ingress
```
- **metallb** to install metalLB loadbalancer with VIP range 192.168.1.200 - 192.168.1.210
```
microk8s enable metallb:192.168.1.200-192.168.1.210
```
- **dashboard** to display the kubernetes dashboard (optional)
```
microk8s enable dashboard
```
- **registry** to store container images locally (optional, useful to store built images)
```
microk8s enable registry
```
