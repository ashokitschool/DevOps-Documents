## Step-1 : Setup Linux VM (Amazon Linux AMI)

1) Login into AWS Cloud account
2) Launch Linux VM using EC2 service   
     - AMI : Amazon Linux
     - Instance Type : t2.medium       
4) Connect to VM using MobaXterm

## Step-2 : Install Docker In Linux VM

```
sudo yum update -y 
sudo yum install docker -y
sudo service docker start
sudo usermod -aG docker ec2-user

exit
```
## Step-3 : Rress 'R' to restart the session (This is in MobaXterm)

## Step-4 :  Verify docker installation
```
docker -v
```
## Step-5 : Run Nexus using docker image
```
docker run -d -p 8081:8081 --name nexus sonatype/nexus3
```

## Step-6: Enable 8081 port number in Security Group Inbound Rules & Access Nexus Server
```
 http://public-ip:8081/
```
## Step-7: Get nexus passsword from Docker Container
```
 docker ps
 docker exec -it <container-id> /bin/bash
 cat /nexus-data/admin.password 
```
## Step-8: Login into Nexus Server




