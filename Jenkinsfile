pipeline {
    agent any
    triggers {
        cron '@midnight'
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'develop', url: 'https://github.com/yungsnowx/quarkus-jenkins-argocd'
            }
        }
        stage('Build') {
            steps {
                sh './mvnw package -DskipTests=true'
            }
        }
        stage('Test') {
            steps {
                sh './mvnw test'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("yungsnow/quarkus-jenkins-argocd:nightly")
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'DOCKERHUB_USERNAME', passwordVariable: 'DOCKERHUB_PASSWORD')]) {
                        docker.withRegistry('https://index.docker.io/v1/', 'dockerhub') {
                            docker.image("yungsnow/quarkus-jenkins-argocd:nightly").push()
                        }
                    }
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