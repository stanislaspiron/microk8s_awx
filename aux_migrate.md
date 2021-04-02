# Migrate configuration from AWX-RPM to AWX 18.0.0 on microk8s

source : https://geekdudes.wordpress.com/2019/12/12/upgrading-migrating-awx-ansible-on-centos-7/

## Install awx-cli
```
pip3 install ansible-tower-cli --upgrade
```

## Export configuration

### Set variables
```
AWX_OLD=http://tower-old.demo.local
PASSWORD_OLD='myOldPassword'
```

### Export configurations to files

```
awx-cli receive -u admin -p $PASSWORD_OLD -h $AWX_OLD --credential all > credential.json
awx-cli receive -u admin -p $PASSWORD_OLD -h $AWX_OLD --credential_type all > credential_type.json
awx-cli receive -u admin -p $PASSWORD_OLD -h $AWX_OLD --project all > project.json
awx-cli receive -u admin -p $PASSWORD_OLD -h $AWX_OLD --inventory all > inventory.json
awx-cli receive -u admin -p $PASSWORD_OLD -h $AWX_OLD --job_template all > job_template.json
awx-cli receive -u admin -p $PASSWORD_OLD -h $AWX_OLD --workflow all > workflow.json
```

## Import configuration

### Set variables
```
AWX_NEW=https://tower.demo.local
PASSWORD_NEW='myOldPassword'
```

### Import configuration files to new AWX

#### Restore Credential types
```
awx-cli send -u admin -p $PASSWORD_NEW -h $AWX_NEW credential_type.json
```

#### Restore Credentials 
```
awx-cli send -u admin -p $PASSWORD_NEW -h $AWX_NEW credential.json
```

Please note that passwords won't be exported, you'll need to enter it manually after usernames are imported (Credentials section in Web GUI)

#### restore project
If projects are defined with credential, reset password manually before importing project.
```
awx-cli send -u admin -p $PASSWORD_NEW -h $AWX_NEW project.json
```

#### restore inventories
```
awx-cli send -u admin -p $PASSWORD_NEW -h $AWX_NEW inventory.json
```

#### before restoring job templates, remove line which contains string credential

Why ? I didn't use this command.
```
sed -i '/\bcredential\b/d' job_template.json
```
#### restore job templates
```
awx-cli send -u admin -p $PASSWORD_NEW -h $AWX_NEW job_template.json
```

#### restore workflows 
```
awx-cli send -u admin -p $PASSWORD_NEW -h $AWX_NEW workflow.json
```
