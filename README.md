End-to-End CI/CD Pipeline with Jenkins, Maven, SonarQube, Docker & AWS EKS
I just completed  a fully automated CI/CD pipeline for a Java web application, covering everything from source code to production deployment on Kubernetes

Tools I used to build during the project include:
Jenkins | Maven | SonarQube | Docker | Trivy | AWS EKS | Kubernetes | ArgoCD | GitHub | GitOps
Key highlights:
•	Zero manual deployments — every code push triggers a full build, test, scan, and deploy cycle
•	GitOps approach keeps the deployment state version-controlled in Git
•	SonarQube quality gate and Trivy security scan run on every build before any image is pushed

Steps to do th eproject.
spin 3 servers for jenkins ,Sonarqube and  EKS 
Run the following command in the jenkins (server1): 
Install the dependancy
```
## Install Java
sudo apt update
sudo apt upgrade
sudo nano /etc/hostname
sudo init 6
sudo apt install openjdk-17-jre
java -version

```

## Install Jenkins
```
#!/bin/bash
Refer--https://www.jenkins.io/doc/book/installing/linux/
curl -fsSL https://pkg.jenkins.io/debian/jenkins.io-2023.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins
```

```
sudo systemctl enable jenkins       //Enable the Jenkins service to start at boot
sudo systemctl start jenkins        //Start Jenkins as a service
systemctl status jenkins
sudo nano /etc/ssh/sshd_config
sudo service sshd reload
ssh-keygen OR $ ssh-keygen -t ed25519
cd .ssh
condigure the jenkins so you can get the GUI
```
```
$ install docker in the jenkins server if the size is large or you can use a slave to do the job .
sudo apt install docker.io -y
## server 2
## install sonarqube
## install docker in the server then use docker to pull sonarqube from docker hub repository.
```
```
sudo apt install docker.io -y
docker run -d -p 8081:8081 sonarqube
## this will pull docker image and run it at the same time
## configure the sonarqube .


