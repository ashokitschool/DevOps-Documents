## Step-1 : Setup Linux VM (Amazon Linux AMI)

1) Login into AWS Cloud account
2) Create Linux VM and connect to it using MobaXterm

## Step-2 : Install Docker In Amazon Linux VM

```
sudo yum update -y 
sudo yum install docker -y
sudo service docker start
```
## Step-3 : Add ec2-user to docker group by executing below command

sudo usermod -aG docker ec2-user

## Step-4 : Restart the session
exit

## Step-5 : Then press 'R' to restart the session (This is in MobaXterm)

## Step-6 :  Verify docker installation
docker -v
