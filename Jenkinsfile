pipeline {
    agent { label 'jenkins-agent' }

    tools {
        jdk 'jdk17'
        maven 'maven3'
    }

    environment {
        SCANNER_HOME = tool 'sonarqube-scannar'  // tool name from Global Tool Configuration
    }

    stages {
        stage('cleanWS') {
            steps {
                cleanWs()
            }
        }

        stage('git checkout') {
            steps {
                git branch: 'main',
                    credentialsId: 'github-cred',
                    url: 'https://github.com/Smartflix/CICD-Project-with-Jenkins-Maven-SonaQube-Docker-and-EKS.git'
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

        stage('CQA') {
            steps {
                withSonarQubeEnv('sonar-scanner') {   // server name from Configure System -> SonarQube servers
                    sh """
                        "${SCANNER_HOME}/bin/sonar-scanner" \
                          -Dsonar.projectName=register-app-pipeline \
                          -Dsonar.projectKey=register-app-pipeline \
                          -Dsonar.java.binaries=webapp/target \
                          -Dsonar.java.source=17
                    """
                }
            }
        }
        stage('build & tag docker image') {
            steps {
                script{
                  withDockerRegistry(credentialsId: 'dockerhub-cred') {
                     sh 'docker build -t fabulousjeff2009/register-app:latest .'  
                    }  
               
                }
            }
        }
    }
}
