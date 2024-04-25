pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                checkout scm
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
                echo 'Deploying....'
            }
        }
    }
    post {
        success {
            deleteDir()
        }
    }
}