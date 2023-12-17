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
## Step-5 : Run SonarQube using docker image
```
docker run -d --name sonarqube -p 9000:9000 -p 9092:9092 sonarqube:lts-community
```

## Step-6: Enable 9000 port number in Security Group Inbound Rules & Access Sonar Server
```
 - URL : http://public-ip:9000/
```
