## Install Docker In Amazon Linux VM

```
$ sudo yum update -y 
$ sudo yum install docker -y
$ sudo service docker start
```
## add ec2-user to docker group by executing below command
$ sudo usermod -aG docker ec2-user

## Restart the session
$ exit

## Then press 'R' to restart the session (This is in MobaXterm)

# Verify docker installation
$ docker -v
