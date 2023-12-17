## Step-1 : Setup Linux VM (Amazon Linux / Ubuntu)

1) Login into AWS Cloud account
2) Create Linux VM and connect to it using MobaXterm

## Step-2 : Install Docker In Amazon Linux VM

```
sudo yum update -y 
sudo yum install docker -y
sudo service docker start
sudo usermod -aG docker ec2-user
exit
```
## Step-3 :  Verify docker installation
docker -v
