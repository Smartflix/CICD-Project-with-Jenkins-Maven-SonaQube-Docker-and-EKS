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
$ install docker in the jenkins server if the size is large or you can use a slave to do the job .
sudo apt install docker.io -y
```
```

## server 2
## install sonarqube
## install docker in the server then use docker to pull sonarqube from docker hub repository.
```
```
sudo apt install docker.io -y
docker run -d -p 8081:8081 sonarqube
## this will pull docker image and run it at the same time
## configure the sonarqube .

```
```
## Third server should be use to bootstap EKS
============================================================= Setup Bootstrap Server for eksctl and Setup Kubernetes using eksctl =============================================================
## Install AWS Cli on the above EC2
Refer--https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html
sudo su
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
apt install unzip,   $ unzip awscliv2.zip
sudo ./aws/install
         OR
sudo yum remove -y aws-cli
pip3 install --user awscli
sudo ln -s $HOME/.local/bin/aws /usr/bin/aws
aws --version

## Installing kubectl
Refer--https://docs.aws.amazon.com/eks/latest/userguide/install-kubectl.html
sudo su
curl -O https://s3.us-west-2.amazonaws.com/amazon-eks/1.27.1/2023-04-19/bin/linux/amd64/kubectl
ll , $ chmod +x ./kubectl  //Gave executable permisions
mv kubectl /bin   //Because all our executable files are in /bin
kubectl version --output=yaml

## Installing  eksctl
Refer---https://github.com/eksctl-io/eksctl/blob/main/README.md#installation
curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
cd /tmp
ll
sudo mv /tmp/eksctl /bin
eksctl version

Setup Kubernetes using eksctl
Refer--https://github.com/aws-samples/eks-workshop/issues/734
eksctl create cluster --name virtualtechbox-cluster \
--region us-east-1 \
--node-type t2.small \
--nodes 3 \

$ kubectl get nodes

```
============================================================= ArgoCD Installation on EKS Cluster and Add EKS Cluster to ArgoCD =============================================================
1 ) First, create a namespace
    $ kubectl create namespace argocd

2 ) Next, let's apply the yaml configuration files for ArgoCd
    $ kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

3 ) Now we can view the pods created in the ArgoCD namespace.
    $ kubectl get pods -n argocd

4 ) To interact with the API Server we need to deploy the CLI:
    $ curl --silent --location -o /usr/local/bin/argocd https://github.com/argoproj/argo-cd/releases/download/v2.4.7/argocd-linux-amd64
    $ chmod +x /usr/local/bin/argocd

5 ) Expose argocd-server
    $ kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'

6 ) Wait about 2 minutes for the LoadBalancer creation
    $ kubectl get svc -n argocd

7 ) Get pasword and decode it.
    $ kubectl get secret argocd-initial-admin-secret -n argocd -o yaml
    $ echo WXVpLUg2LWxoWjRkSHFmSA== | base64 --decode

## Add EKS Cluster to ArgoCD
9 ) login to ArgoCD from CLI
    $ argocd login a2255bb2bb33f438d9addf8840d294c5-785887595.ap-south-1.elb.amazonaws.com --username admin

10 ) 
     $ argocd cluster list

11 ) Below command will show the EKS cluster
     $ kubectl config get-contexts

12 ) Add above EKS cluster to ArgoCD with below command
     $ argocd cluster add i-08b9d0ff0409f48e7@virtualtechbox-cluster.ap-south-1.eksctl.io --name virtualtechbox-eks-cluster

13 ) $ kubectl get svc
============================================================= Cleanup =============================================================
$ kubectl get all
$ kubectl delete deployment.apps/virtualtechbox-regapp       //it will delete the deployment
$ kubectl delete service/virtualtechbox-service              //it will delete the service
$ eksctl delete cluster virtualtechbox --region ap-south-1     OR    eksctl delete cluster --region=ap-south-1 --name=virtualtechbox-cluster      //it will delete the EKS cluster

