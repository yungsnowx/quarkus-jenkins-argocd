pipeline {
    agent any
    stages {
        stage('Clone') {
            steps {
                git branch: 'main', url: 'https://github.com/yungsnowx/quarkus-jenkins-argocd'
            }
        }
        stage('Build') {
            steps {
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