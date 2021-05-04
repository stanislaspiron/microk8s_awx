# Execution environment container creation
The goal is to create dedicated Execution Environment to use in AWX version 18.0.0 and above.  
The following procedure describe how to create EE image and push to microk8s registry for lab purpose.

## First time configuration
Source : https://microk8s.io/docs/registry-built-in
### **Host : microk8s**
start container registry on microk8s
```
microk8s enable registry
```

### **Host : Image builder host**
In my configuration, docker is installed in a dedicated host.

System Requirement :
- docker

Allow HTTP service for registry in docker host. we can define hostname or IP address.
```
cat <<EOF > /etc/docker/daemon.json
{
  "insecure-registries" : ["192.168.1.8:32000"],
  "insecure-registries" : ["microk8s.demo.local:32000"]
}
EOF
```

Restart docker service.
```
sudo systemctl restart docker
```

## Execution environment container creation
Source: https://github.com/ansible/awx/issues/10060
### **Host : Image builder host**
prepare directory for building
```
mkdir awx-ee-custom
cd awx-ee-custom/
```
Create EE specifications file
```
cat <<EOF > Dockerfile
FROM quay.io/ansible/awx-ee:0.1.1

USER root

ADD requirements.yml /tmp/requirements.yml
ADD requirements.txt /tmp/requirements.txt

RUN ansible-galaxy collection install -r /tmp/requirements.yml --collections-path /usr/share/ansible/collections
RUN pip --disable-pip-version-check install -r /tmp/requirements.txt  # for any Python modules the collections didn't pull in

USER 1000
```
create collections requirements file
```
cat <<EOF > requirements.yml
---
collections:
# With just the collection name
- f5networks.f5_modules
- community.general
EOF
```

create python modules requirements file
```
cat <<EOF > requirements.txt
dnspython
EOF
```

Build container with docker (podman failed to create image because of /usr rights)
```
docker build -t microk8s.demo.local:32000/awx-ee-custom:1.1.0 .
```
Optional parameters to support proxy (apt command requires lowercase variable names)
```
--build-arg "HTTP_PROXY=$HTTP_PROXY" --build-arg "HTTPS_PROXY=$HTTPS_PROXY"  --build-arg "http_proxy=$HTTP_PROXY" --build-arg "https_proxy=$HTTPS_PROXY"
```

push image to microk8s registry
```
docker push microk8s.demo.local:32000/awx-ee-custom
```
