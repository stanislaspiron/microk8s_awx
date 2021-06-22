# How to deploy AWX in microk8s

## What you’ll need (not tested with other microk8s specifications)
- An Ubuntu 20.04 LTS environment to run the commands
- At least 60G of disk space and 8G of memory are recommended
- An internet connection


# Microk8s installation with AWX

1. [Install microk8s](microk8s/microk8s_install.md)
1. [Install AWX](awx/awx_install.md)

Optional configuration :
- [configure dashboard ingress](microk8s/dashboard_install.md)
- [export / import configuration from existing AWX](awx/awx_migrate.md)
- [configure NFS storage class](nfs/README.md)

Debug:
- If you can't authenticate, you can [manage AWX account](awx/awx_account_management.md)
- If your microk8s is behind a proxy, you must [configure proxy configuration](microk8s/microk8s_proxy.md)

# AWX custom Execution environment

1. [create EE image and push to microk8s registry](EE/ansible-ee-building.md)
1. [Configure AWX to use custom EE](EE/awx-ee-container-group.md)
1. [Configure AWX EE with persistent volume](EE/awx-ee-container-group-pvc.md)


## Addtionnal informations
When this procedure was written, following version was installed

- microk8s : v1.20
- awx : 
  - 18.0.0 (requires operator 0.7.0, awx.yml file format changed since operator 0.11.0)
  - 19.0.0 (requires operator 0.8.0, awx.yml file format changed since operator 0.11.0)
  - 19.1.0 (requires operator 0.9.0, awx.yml file format changed since operator 0.11.0)
  - 19.2.1
