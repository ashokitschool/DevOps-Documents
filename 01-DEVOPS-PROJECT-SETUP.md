# DevOps Project Setup with CI CD Pipeline

1) Maven
2) Git Hub
3) Tomcat
4) SonarQube
5) Nexus Repo
6) Jenkins

# Step-1 : Create github stage 

stage('clone repo') { <br/>          
   git credentialsId: 'GIT-Credentials', url: 'https://github.com/ashokitschool/maven-web-app.git' <br/>
}

# Step-2 : Create Maven Build Stage (Add maven in global tools)

stage ('Maven Build') { <br/>
        def mavenHome = tool name: "Maven-3.9.4", type: "maven" <br/>
        def mavenCMD = "${mavenHome}/bin/mvn" <br/>
        sh "${mavenCMD} clean package" <br/>
}


# Step-3 : Create SonarQube stage

a) Start Sonar Server <br/>
b) Login into Sonar Server & Generate Sonar Token  <br/>
	Ex: cedbc0b89e45c58f4a86e4687f2df2a2241e3369 <br/>
c) Add Sonar Token in 'Jenkins Credentials' as Secret Text <br/>
			-> Manager Jenkins  <br/>
			-> Credentials  <br/>
			-> Add Credentials <br/>
			-> Select Secret text <br/>
			-> Enter Sonar Token as secret text  <br/>

d) Install SonarQube Scanner Plugin <br/>
-> Manage Jenkins -> Plugins -> Available -> Sonar Qube Scanner Plugin -> Install it

e) Configure SonarQube Server <br/>
-> Manage Jenkins -> Configure System -> Sonar Qube Servers -> Add Sonar Qube Server 
		
				- Name : Sonar-Server-7.8 <br/>
				- Server URL : http://52.66.247.11:9000/   (Give your sonar server url here) <br/>
				- Add Sonar Server Token <br/>			

f) Add SonarQube Stage in Jenkins Pipeline

stage('SonarQube analysis') {
			withSonarQubeEnv('Sonar-Server-7.8') {
			def mavenHome = tool name: "Maven-3.8.6", type: "maven"
			def mavenCMD = "${mavenHome}/bin/mvn"
			sh "${mavenCMD} sonar:sonar"
    	}
}


# Step-4 : Create Nexus Stage

a) Run nexus VM and create nexus repository
b) Create Nexus Repository 
c) Install Nexus Repository Plugin using Manage Plugins   ( Plugin Name : Nexus Artifact Uploader)
d) Generate Nexus Pipeline Syntax

stage ('Nexus Upload'){
nexusArtifactUploader artifacts: [[artifactId: '01-Maven-Web-App', classifier: '', file: 'target/01-maven-web-app.war', type: 'war']], credentialsId: 'Nexus-Credentials', groupId: 'in.ashokit', nexusUrl: '13.127.185.241:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'ashokit-snapshot-repository', version: '1.0-SNAPSHOT'
}


# Step-5 : Create Deploy Stage

-> Start Tomcat Server
-> Install SSH Agent plugin using Manage Plugins
-> Generate SSH Agent and configure stage
-> Add Tomcat Server as 'Uname with Secret Text'
stage ('Deploy'){
       sshagent(['Tomcat-Server-Agent']) {
		sh 'scp -o StrictHostKeyChecking=no target/01-maven-web-app.war ec2-user@15.207.100.83:/home/ec2-user/apache-tomcat-9.0.80/webapps'
	   }
}
