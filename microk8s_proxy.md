# Microk8s Proxy configuration

source: https://microk8s.io/docs/install-proxy

If Microk8s is behind a proxy, you must:
- configure proxy configuration
- exclude internal networks for proxy (at least internal k8s cluster network)

All those configurations are in **/var/snap/microk8s/current/args/containerd-env**

## Proxy configuration:

```
HTTP_PROXY=http://1.2.3.4:8888
HTTPS_PROXY=http://1.2.3.4:8888
```

## Proxy exclusion
When enabling proxy configuration, you should at least exclude 10.152.183.0/24 network (default mkcrok8s internal network)
```
NO_PROXY=10.1.0.0/16,10.152.183.0/24
```
