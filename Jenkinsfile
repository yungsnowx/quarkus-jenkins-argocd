pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                sh 'pwd'
                sh 'echo passed!!!!'
                git branch: 'main', url: 'https://github.com/yungsnowx/quarkus-jenkins-argocd'
                sh 'pwd'
                sh 'ls'
            }
        }
    }
}