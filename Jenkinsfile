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
        stage('Build and Test') {
            steps {
                sh 'ls -ltr'
                // build the project and create a JAR files
                sh 'cd quarkus-jenkins-argocd && mvn clean package'
            }
        }
    }
}