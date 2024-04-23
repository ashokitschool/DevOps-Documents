# Ansible Setup in Amazon Linux VMs #

## Step-0: Create 3 Amazon Linux VMs in AWS (Free Tier Eligible - t2.micro)

1 - Control Node <br/>
2 - Managed Nodes

Note: Connect to all 3 VMs using MobaXterm

## Step-1: Execute below commands in all 3 VMs ##

### a) Create user ###
```
sudo useradd ansible
sudo passwd ansible
```
### b) Configure user in sudoers file ###

sudo visudo

```
ansible ALL=(ALL) NOPASSWD: ALL
```

### c) Update sshd config file ###
```
sudo vi /etc/ssh/sshd_config
```
-> comment PasswordAuthentication no
-> PermitEmptyPasswords yes

### d) Restart the server ###
```   
sudo service sshd restart
```
Note: Do the above steps in all the 3 machines 

## Step-2: Install Ansible in Control Node ##

### a) Switch to Ansible user ### 
```
sudo su ansible
cd ~
```
### b) Install Python ###
```
  sudo yum install python3 -y
```
### c) Check python version  ###
```
python3 --version
```
### d) Install PIP (It is a python package manager) ###
```
sudo yum -y install python3-pip
```
### e) Install Ansible using Python PIP ###
```
pip3 install ansible --user
```
### f) Verify ansible version  ###
```
ansible --version
```
### g)  Create ansible folder under /etc ###
```
sudo mkdir /etc/ansible 
```

## Step-3: Generate SSH Key In Control Node and  Copy SSH key into Managed Nodes ##

### a) Switch to ansible user ###

```
sudo su ansible
```
### Generate ssh key using below command ###
```
ssh-keygen
```
### b) Copy it to Managed Nodes as ansible user ###
```
ssh-copy-id ansible@<ManagedNode-Private-IP>
```
Ex : $ ssh-copy-id ansible@172.31.44.90
 
Note: Repeat above command by updating HOST IP for all the managed Servers.

## Step-4: Update Host Inventory in Ansible Server to add managed node servers details ##
```
sudo vi /etc/ansible/hosts
```
[webservers] <br/>
172.31.47.247
<br/>
[dbservers] <br/>
172.31.44.90

## Step-5: Test Connectivity ##
```
ansible all -m ping
```
