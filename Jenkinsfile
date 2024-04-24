pipeline {
    agent any
    stages {
        stage('Clone') {
            steps {
                git branch: 'main', url: 'https://github.com/yungsnowx/quarkus-jenkins-argocd'
                sh 'ls'
            }
        }
        stage('Build') {
            steps {
                sh 'ls -ltr'
                sh './mvnw clean package'
            }
        }
    }
    post {
        success {
            deleteDir()
        }
    }
}