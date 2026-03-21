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
                withSonarQubeEnv('sonar-scanner') {
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

        stage('Debug user on agent') {
            steps {
                sh '''
                  echo "WHOAMI:" && whoami
                  echo "ID:" && id
                  echo "GROUPS:" && groups
                  echo "HOSTNAME:" && hostname
                  echo "DOCKER PS:" && docker ps || echo "docker ps failed"
                '''
            }
        }

        stage('build & tag docker image') {
            steps {
                script {
                    withDockerRegistry(
                        credentialsId: 'dockerhub-cred',
                        url: 'https://index.docker.io/v1/'
                    ) {
                        sh 'docker build -t fabulousjeff2009/register-app:latest .'
                    }
                }
            }
        }
         stage('Trivy Image Scan') {
            steps {
                sh 'trivy image --format table -o image.html fabulousjeff2009/register-app:latest'
            }
        }
    }
}
