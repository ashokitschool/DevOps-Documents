## Setup Linux VM (Amazon Linux / Ubuntu)

1) Login into AWS Cloud account
2) Create Linux VM and connect to it using MobaXterm

## Install Docker In Amazon Linux VM

```
sudo yum update -y 
sudo yum install docker -y
sudo service docker start
sudo usermod -aG docker ec2-user
exit
```
## Install Docker In Ubuntu VM

```
sudo apt update
curl -fsSL get.docker.com | /bin/bash
sudo usermod -aG docker ubuntu 
exit
```

## Verify docker installation

```
docker -v
```
