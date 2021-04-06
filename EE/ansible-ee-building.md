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
In my configuration, image buider is installed in a dedicated host.

System Requirement :
- docker
- pip3

Install ansible builder
```
pip3 install ansible-builder
```

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
Source: https://www.ansible.com/blog/introduction-to-ansible-builder
### **Host : Image builder host**
prepare directory for building
```
mkdir awx-ee-f5
cd awx-ee-f5/
```
Create EE specifications file
```
cat <<EOF > execution-environment.yml
---
version: 1
dependencies:
  galaxy: requirements.yml
additional_build_steps:
  prepend: |
    RUN pip3 install --upgrade pip setuptools
EOF
```
create collections requirements file
```
cat <<EOF > requirements.yml
---
collections:
# With just the collection name
- f5networks.f5_modules
EOF
```

Build container with docker (podman failed to create image because of /usr rights)
```
ansible-builder build --tag microk8s.demo.local:32000/awx-ee-f5:1.0.0 --context ./context --container-runtime docker
```

push image to microk8s registry
```
docker push microk8s.demo.local:32000/awx-ee-f5
```
