# Ansible Installation of Ansible-Server
 
# To set host name in all nodes 
```
hostnamectl set-hostname <Your_host_name>
```
# Installation of ansible
```
wget https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
yum install epel-release-latest-7.noarch.rpm -y 
yum update -y 
yum install git python python-level python.pip openssl ansible -y 
```
# Check version
```
ansible --version 
```
# Bellow line number 11 add one group in [] and private ip-addresses of worker-nodes
```
vi /etc/ansible/hosts  
```
# As like bellow. 
```
[group-name]
private ip of node-1 
private ip of node-2 
```
# Do few changes in this file
```
vi /etc/ansible/ansible.cfg   
```
# Uncomment bellow lines.
1. inventory = /etc/ansible/hosts
2. sudo-user = root 
# Now create one user, in all the three instances 
```
adduser ansible  
```
# Give password to in machine
```
passwd ansible  
```
# Line number 101 add ansible user in all node
```
visudo 
```
```
ansible ALL=(ALL) NOPASSWD: ALL 
```
# Do it in all service node and other node
```
vi /etc/ssh/sshd_config    
```
# Uncomment line number 38, 61 and Comment the line number 63

38. PermitRootLogin yes        
.
61. PasswordAuthentication yes   

# Comment 

63. PasswordAuthentication no   
# Start sshd service.
```
service sshd restart 
```
# Switch user in all node.
```
su - ansible 
```
# We can access node
```
ssh <private of node>  
```
# Now go to ansible server and create keys for ansible user. 
```
ssh-keygen
cd .ssh 
ls
```
# Send Public key to All worker nodes.
```
ssh-copy-id ansible@<private ip of node> 
```
# Enter passwor of node ansible user 

# Go to Mater Node and check hosts with following command. 
```
ansible all --list-hosts 
```
# To check groups.
```
ansible <group-name> --list-hosts  
```
Ad-hoc Commands 
```
ansible all/<group-name> -a "ls" 
```
# Simple Site Hosting.
```
ansible <group-name> -a "sudo yum install httpd -y"
ansible <group-name> -a "systemctl start httpd"
```
# Hit public Ip of worker Node you will see "It's Work" page of httpd.

# For remove httpd sevice.
```
ansible <group-name> -ba "yum remove httpd -y"    
```
