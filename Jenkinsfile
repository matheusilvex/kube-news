pipeline{
    agent any
    
    stages{
        stage('Build Docker-Image'){
            steps{ 
                script{
                    dockerapp = docker.build("matheusilvex/kube-news:${env.BUILD_ID}", '-f ./src/Dockerfile ./src')
                }                    
            }
        }

        stage('Push Docker Image'){
            steps{
                script{
                    docker.withRegistry("https://registry.hub.docker.com","dockerhub")
                        dockerapp = docker.push("latest")
                        dockerapp = docker.push("${env.BUILD_ID}")

                }
            }
        }
    }
}