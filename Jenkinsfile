pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "i211185sameer/flask-app:latest"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/umerfaro/Mlops_Assignment-_1.git']])
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build(env.DOCKER_IMAGE)
                }
            }
        }
        stage('Push to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub-credentials') {
                        dockerImage.push()
                    }
                }
            }
        }
    }
    post {
        success {
              emailext (
                  to: 'i211185@nu.edu.pk',
                  subject: "Deployment Successful",
                body: "The master branch has been successfully deployed via Jenkins.",
                  mimeType: 'text/html'
              )
          }
          failure {
              emailext (
                  to: 'i211184@nu.edu.pk',
                  subject: "Deployment Failed",
                body: "There was an issue deploying the master branch. Please check the Jenkins logs.",
                  mimeType: 'text/html'
              )
          }
    }
}