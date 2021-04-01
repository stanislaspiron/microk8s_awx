# How to deploy AWX in microk8s

## What you’ll need (not tested with other microk8s specifications)
- An Ubuntu 20.04 LTS environment to run the commands
- At least 60G of disk space and 8G of memory are recommended
- An internet connection


# Microk8s installation with AWX

1. [Install microk8s](microk8s_install.md)
1. [Install AWX](awx_install.md)
1. [Optional : configure dashboard ingress](dashboard_install.md)
 
# AWX custom Execution environment

1. [create EE image](ansible-ee-building.md)
1. [Configure AWX to use custom EE](awx-ee.md)



## Addtionnal informations
When this procedure was written, following version was installed

- microk8s : v1.20
- awx : 18.0.0
