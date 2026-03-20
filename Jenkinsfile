pipeline {
    agent { label 'jenkins-agent' }

    tools {
        jdk 'jdk17'
        maven 'maven3'
    }

    environment {
        SCANNER_HOME = tool 'sonarqube-scannar'
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
                withSonarQubeEnv('soner-scanner') {   // Name must match SonarQube server in Configure System
                    sh """
                        "${SCANNER_HOME}/bin/sonar-scanner" \
                          -Dsonar.projectName=register-app-pipeline \
                          -Dsonar.projectKey=register-app-pipeline \
                          -Dsonar.java.binaries=target \
                          -Dsonar.java.source=17
                    """
                }
            }
        }
    }
}

