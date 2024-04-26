pipeline {
    agent any
    tools {
        maven 'Maven-3.9.6'
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/yungsnowx/quarkus-jenkins-argocd'
            }
        }
        stage('Build') {
            steps {
                sh './mvnw package -DskipTests=true'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("yungsnow/quarkus-jenkins-argocd:latest")
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    docker.image("yungsnow/quarkus-jenkins-argocd:latest").push()
                }
            }
        }
    }
    post {
        success {
            deleteDir()
        }
    }
}