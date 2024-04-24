pipeline {
    agent any
    tools {
        docker "docker"
    }
    stages {
        stage('Clone') {
            steps {
                git branch: 'main', url: 'https://github.com/yungsnowx/quarkus-jenkins-argocd'
            }
        }
        stage('Build') {
            steps {
                sh './mvnw clean package'
                sh 'docker build -t yungsnow/quarkus-jenkins-argocd .'
            }
        }
        stage('Package') {
            steps {
                withCredentials([string(credentialsId: 'dockerhub_yungsnow_token', variable: 'dockerhub_yungsnow_token')]) {
                    sh 'docker login -u yungsnow -p ${dockerhub_yungsnow_token}'
                    sh 'docker push yungsnow/quarkus-jenkins-argocd'
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