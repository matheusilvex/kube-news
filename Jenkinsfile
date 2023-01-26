pipeline{
    agent any
    
    stages{
        stage('Build Docker-Image'){
            script{
                dockerapp = docker.buid("matheusilvex/kube-news:${env.BUILD_ID}", '-f ./src/Dockerfile ./src')
            }
        }
    }
}