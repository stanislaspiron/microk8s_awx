# AWX Account management
If after installation, you can't log in AWX, you can :
1. Create a new user
2. Change admin password

Source: https://www.reddit.com/r/ansible/comments/m5nd1r/ansible_awx_fresh_install_no_administrator/
## Log in <resource name>-web container
in the following example, my awx resource name is tower.
```
root@srv-p-asu3-1m:~# kubectl get pods -n awx
NAME                           READY   STATUS    RESTARTS   AGE
awx-operator-57bcb58f5-d9gdh   1/1     Running   0          40h
tower-postgres-0               1/1     Running   0          20m
tower-8485d5d74f-6gqq7         4/4     Running   0          20m
root@srv-p-asu3-1m:~# kubectl exec -n awx -it tower-8485d5d74f-6gqq7 -c tower-web -- bin/bash
bash-4.4$
```

## Create new user
```
bash-4.4$ awx-manage createsuperuser
Username (leave blank to use 'awx'): my_new_user
Email address:
Password:
Password (again):
Superuser created successfully.
```
## Change admin password
  
```
bash-4.4$ awx-manage update_password --username admin --password 'myNewPassword'
Password updated
```
