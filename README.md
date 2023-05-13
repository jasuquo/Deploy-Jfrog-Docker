# Deploy-Jfrog-Docker with Ansible
### Prequisites fopr this deploy ensure you hve ansible set up and configured 
### create your ansible inventory file with your server blocks defined example below 
```
[myservers]
192.168.198.10
192.888.222.11
```
### test your connection to the servers by ensuring your ssh pub keys are on the servers 

### deploy the playbook 
```
ansible-playbook playbookname.yml -i inventory
```
