# Project Setup using below tools
1) Maven
2) Git Hub
3) Jenkins
4) Docker
5) Kubernetes

# Step-1 : Jenkins Server Setup in Linux VM #

1) Create Ubuntu VM using AWS EC2 (t2.medium) <br/>
2) Enable 8080 Port Number in Security Group Inbound Rules
3) Connect to VM using MobaXterm
4) Instal Java

```
sudo apt update
sudo apt install fontconfig openjdk-17-jre
java -version
```

3) Install Jenkins
```
sudo wget -O /usr/share/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins
```
4) Start Jenkins

```
sudo systemctl enable jenkins
sudo systemctl start jenkins
```

5) Verify Jenkins

```
sudo systemctl status jenkins
```
	
6) Open jenkins server in browser using VM public ip

```
http://public-ip:8080/
```

7) Copy jenkins admin pwd
```
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```
	   
8) Create Admin Account & Install Required Plugins in Jenkins


## Step-2 : Configure Maven as Global Tool in Jenkins ##
1) Manage Jenkins -> Tools -> Maven Installation -> Add maven <br/>

## Step-3 : Setup Docker in Jenkins ##
```
curl -fsSL get.docker.com | /bin/bash
sudo usermod -aG docker jenkins
sudo systemctl restart jenkins
sudo docker version
```

# Step - 4 : Create EKS Management Host in AWS #

1) Launch new Ubuntu VM using AWS Ec2 ( t2.micro )	  
2) Connect to machine and install kubectl using below commands  
```
curl -o kubectl https://amazon-eks.s3.us-west-2.amazonaws.com/1.19.6/2021-01-05/bin/linux/amd64/kubectl <br/>
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin
kubectl version --short --client
```
3) Install AWS CLI latest version using below commands 
```
sudo apt install unzip 
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
aws --version
```

4) Install eksctl using below commands
```
curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
sudo mv /tmp/eksctl /usr/local/bin
eksctl version <br/>
```

# Step - 5 : Create IAM role & attach to EKS Management Host & Jenkins Server #

1) Create New Role using IAM service ( Select Usecase - ec2 ) 	
2) Add below permissions for the role <br/>
	- IAM - fullaccess <br/>
	- VPC - fullaccess <br/>
	- EC2 - fullaccess  <br/>
	- CloudFomration - fullaccess  <br/>
	- Administrator - acces <br/>
		
3) Enter Role Name (eksroleec2) 
4) Attach created role to EKS Management Host (Select EC2 => Click on Security => Modify IAM Role => attach IAM role we have created) 
5) Attach created role to Jenkins Machine (Select EC2 => Click on Security => Modify IAM Role => attach IAM role we have created) 
  
# Step - 6 : Create EKS Cluster using eksctl # 
**Syntax:** 

eksctl create cluster --name cluster-name  \
--region region-name \
--node-type instance-type \
--nodes-min 2 \
--nodes-max 2 \ 
--zones <AZ-1>,<AZ-2>

```
eksctl create cluster --name ashokit-cluster --region ap-south-1 --node-type t2.medium  --zones ap-south-1a,ap-south-1b**
```

Note: Cluster creation will take 5 to 10 mins of time (we have to wait). After cluster created we can check nodes using below command.	
```
kubectl get nodes  
```
# Step - 7 : Install AWS CLI in JENKINS Server #

URL : https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html  

**Execute below commands to install AWS CLI**
```
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
aws --version
```
 
# Step - 8 : Install Kubectl in JENKINS Server #
**Execute below commands in Jenkins server to install kubectl**

```
curl -o kubectl https://amazon-eks.s3.us-west-2.amazonaws.com/1.19.6/2021-01-05/bin/linux/amd64/kubectl
chmod +x ./kubectl
udo mv ./kubectl /usr/local/bin
kubectl version --short --client
```

# Step - 9 : Update EKS Cluster Config File in Jenkins Server #
	
1) Execute below command in Eks Management host & copy kube config file data <br/>
	$ cat .kube/config 

2) Execute below commands in Jenkins Server and paste kube config file  <br/>
	$ cd /var/lib/jenkins <br/>
	$ sudo mkdir .kube  <br/>
	$ sudo vi .kube/config  <br/>

3) Execute below commands in Jenkins Server and paste kube config file for ubuntu user to check EKS Cluster info<br/>
	$ cd ~ <br/>
	$ ls -la  <br/>
	$ sudo vi .kube/config  <br/>

4) check eks nodes <br/>
	$ kubectl get nodes 

**Note: We should be able to see EKS cluster nodes here.**

# Step - 10 : Create Jenkins CI Job #

- **Stage-1 : Clone Git Repo** <br/> 
- **Stage-2 : Build** <br/>
- **Stage-3 : Create Docker Image** <br/>
- **Stage-4 : Push Docker Image to Registry** <br/>
- **Stage-5 : Trigger CD Job** <br/>
	
# Step - 11 : Create Jenkins CD Job #

- **Stage-1 : Clone k8s manifestfiles** <br/>
- **Stage-2 : Deploy app in k8s eks cluster** <br/>
- **Stage-3 : Send confirmatin email** <br/>

	
# Step - 12 : Trigger Jenkins CI Job #
- **CI Job will execute all the stages and it will trigger CD Job** <br/>
- **CD Job will fetch docker image and it will deploy on cluster** <br/>
	
# Step - 13 : Access Application in Browser #
- **We should be able to access our application** <br/>
URL : http://LBR/context-path/
	
# We are done with our Setup #
	
## Step - 14 : After your practise, delete Cluster and other resources we have used in AWS Cloud to avoid billing ##
