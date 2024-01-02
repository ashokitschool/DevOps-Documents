# Ansible Setup in Amazon Linux VMs #

## Step-0: Create 3 Amazon Linux VMs in AWS (Free Tier Eligible - t2.micro)

1 - Control Node <br/>
2 - Managed Nodes

Note: Connect to all 3 VMs using MobaXterm

## Step-1: Execute below commands in all 3 VMs ##

### 1) Create user ###
```
sudo useradd ansible
sudo passwd ansible
```
### 2) Configure user in sudoers file ###

sudo visudo
ansible ALL=(ALL) NOPASSWD: ALL

### 3) Update sshd config file ###
```
sudo vi /etc/ssh/sshd_config
```
-> comment PasswordAuthentication no
-> PermitEmptyPasswords yes

### 4) Restart the server ###
```   
sudo service sshd restart
```
Note: Do the above steps in all the 3 machines 

## Step-2: Install Ansible in Control Node ##

### 1) Switch to Ansible user ### 
```
sudo su ansible
cd ~
```
### 2) Install Python ###
```
  sudo yum install python3 -y
```
### 3) Check python version  ###
```
python3 --version
```
### 4) Install PIP (It is a python package manager) ###
```
sudo yum -y install python3-pip
```
### 5) Install Ansible using Python PIP ###
```
pip3 install ansible --user
```
### 6) Verify ansible version  ###
```
ansible --version
```
### 7)  Create ansible folder under /etc ###
```
sudo mkdir /etc/ansible 
```

## Step-3: Generate SSH Key In Control Node and  Copy SSH key into Managed Nodes ##

### 1) Switch to ansible user ###

```
sudo su ansible
```
### Generate ssh key using below command ###
```
ssh-keygen
```
### 2) Copy it to Managed Nodes as ansible user ###
```
ssh-copy-id ansible@<ManagedNode-Private-IP>
```
Ex : $ ssh-copy-id ansible@172.31.44.90
 
Note: Repeat above command by updating HOST IP for all the managed Servers.

## Step-4: Update Host Inventory in Ansible Server to add managed node servers details ##
```
sudo vi /etc/ansible/hosts
```
[webservers]
172.31.47.247

[dbservers]
172.31.44.90

## Step-5: Test Connectivity ##
```
ansible all -m ping
```
