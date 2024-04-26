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
        stage('Push Docker Image') {
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
        stage('Deploy to Kubernetes') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'argocd-login', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD'), string(credentialsId: 'argocd-server', variable: 'SERVER')]) {
                        sh '''
                        argocd login "${SERVER}" --insecure --username "${USERNAME}" --password "${PASSWORD}"
                        # argocd app actions run quarkus-jenkins-argocd-argo-application restart --kind Deployment
                        argocd app set quarkus-jenkins-argocd-argo-application --image yungsnow/quarkus-jenkins-argocd:nightly
                        argocd app sync quarkus-jenkins-argocd-argo-application
                        '''
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