pipeline {
    agent {label 'jenkins-agent'}
  tools {
        jdk 'jdk17'
        maven 'maven3'
    }
    stages {
        stage("cleanWS") {
            steps {
                cleanWs()
            }
        }

        stage('git checkout') {
            steps {
                git branch: 'main', credentialsId: 'github-cred', url: 'https://github.com/Smartflix/CICD-Project-with-Jenkins-Maven-SonaQube-Docker-and-EKS.git'
            }
        }
       
        stage('compile application') {
            steps {
                sh 'mvn compile'
            }
        }
       
        stage('test application') {
            steps {
                sh 'mvn test'
            }
        }
  
        stage('build application') {
            steps {
                sh 'mvn clean package'
            }
        }
    }
}
