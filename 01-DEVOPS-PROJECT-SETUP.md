# DevOps Project Setup with CI CD Pipeline

1) Git Hub
2) Maven
3) SonarQube
4) Nexus Repo
5) Tomcat
6) Jenkins

## Step-0 : Create Jenkins Pipeline (Scripted)

```
node {

}
```
## Step-1 : Add github stage to clone git repository

1) Use Pipeline Syntax and Generate Script for Git Clone with Repo Details

```     
  git credentialsId: 'GIT-Credentials', url: 'https://github.com/ashokitschool/maven-web-app.git'
```

2) Add Generated Script in Pipeline as a stage

```
 stage('clone repo') {        
  git credentialsId: 'GIT-Credentials', url: 'https://github.com/ashokitschool/maven-web-app.git'
}
```

## Step-2 : Create Maven Build Stage (Add maven in global tools)

1) Configure Maven as Global Tool in Manage Jenkins

2) Add Maven Build Stage in Pipeline
```
 stage ('Maven Build') {
       def mavenHome = tool name: "Maven-3.9.4", type: "maven"
       def mavenCMD = "${mavenHome}/bin/mvn"
       sh "${mavenCMD} clean package"
 }
```

## Step-3 : Add SonarQube stage

1) Start Sonar Server <br/>
2) Login into Sonar Server & Generate Sonar Token  <br/>
	Ex: cedbc0b89e45c58f4a86e4687f2df2a2241e3369 <br/>
3) Add Sonar Token in 'Jenkins Credentials' as Secret Text <br/>
			-> Manager Jenkins  <br/>
			-> Credentials  <br/>
			-> Add Credentials <br/>
			-> Select Secret text <br/>
			-> Enter Sonar Token as secret text  <br/>

4) Install SonarQube Scanner Plugin <br/>
-> Manage Jenkins -> Plugins -> Available -> Sonar Qube Scanner Plugin -> Install it

5) Configure SonarQube Server <br/>
-> Manage Jenkins -> Configure System -> Sonar Qube Servers -> Add Sonar Qube Server 
		- Name : Sonar-Server-7.8
		- Server URL : http://52.66.247.11:9000/   (Give your sonar server url here)
		- Add Sonar Server Token

6) Add SonarQube Stage in Jenkins Pipeline

```
stage('SonarQube analysis') {
	withSonarQubeEnv('Sonar-Server-7.8') {
	def mavenHome = tool name: "Maven-3.8.6", type: "maven"
	def mavenCMD = "${mavenHome}/bin/mvn"
	sh "${mavenCMD} sonar:sonar"
    }
}
```

## Step-4 : Create Nexus Stage

1) Run nexus VM and create nexus repository
2) Create Nexus Repository 
3) Install Nexus Repository Plugin using Manage Plugins   ( Plugin Name : Nexus Artifact Uploader)
4) Generate Nexus Pipeline Syntax
```
stage ('Nexus Upload'){
nexusArtifactUploader artifacts: [[artifactId: '01-Maven-Web-App', classifier: '', file: 'target/01-maven-web-app.war', type: 'war']], credentialsId: 'Nexus-Credentials', groupId: 'in.ashokit', nexusUrl: '13.127.185.241:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'ashokit-snapshot-repository', version: '1.0-SNAPSHOT'
}
```


## Step-5 : Create Deploy Stage

1) Start Tomcat Server <br/>
2) Install SSH Agent plugin using Manage Plugins <br/>
3) Generate SSH Agent and configure stage <br/>
4) Add Tomcat Server as 'Uname with Secret Text' <br/>

```
stage ('Deploy'){ 
sshagent(['Tomcat-Server-Agent']) {
sh 'scp -o StrictHostKeyChecking=no target/01-maven-web-app.war ec2-user@15.207.100.83:/home/ec2-user/apache-tomcat-9.0.80/webapps'
  }
}
```
