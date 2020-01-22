### ICP deploy

These playbooks deploy an ICP cluster by existing ICP boot nodes

### Requires 
- Requires Ansible installed on the trigger node (yum install -y ansible)
- ICP boot nodes is ready
- Finsh below on ansible host
  - yum -y install epel-release
  - yum install python-pip
  - pip install --upgrade pip

### How to use

1. specify the boot nodes in **hosts** file
```
[boot-nodes]
9.111.215.35
9.111.212.130
```
2. specify  the configuration in **all** file
```
# Here are variables related to the ICP installation
cleanup_image: false
code_repository: git@github.ibm.com:IBMPrivateCloud/installer.git
code_directory: /root/src/source/installer

version: latest
image_repo: registry.ng.bluemix.net/mdelder
private_registry_server: registry.ng.bluemix.net
docker_username: token
docker_password: xxxx
```

3. run

- install
```
ansible-playbook -i  hosts install.yml
```

- uninstall:

```
ansible-playbook -i  hosts uninstall.yml --extra-vars "cleanup_image=true"
```
4.  It also has per and post action interface when you need do special things on all ICP cluster host
- pre-install.yaml
- pre-uninstall.yaml
- post-install .yaml
- post-uninstall.yaml

5. enable a cron job to auto re-install ICP
for example: re-install ICP every day at 08:08AM
```
08 08 * * * cd /root/src/ansible-examples/tomcat-standalone && ansible-playbook -i  hosts uninstall.yml --extra-vars "cleanup_image=true" >> $HOME/for_crontab/mylog.log 2>&1 && ansible-playbook -i  hosts install.yml >> $HOME/for_crontab/mylog.log 2>&1
```
