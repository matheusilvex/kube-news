pipeline{
    agent any
    
    stages{
        //Aqui é a parte de CI, Integração Continua
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
                    docker.withRegistry('https://registry.hub.docker.com/','dockerhub'){
                        dockerapp.push('latest')
                        dockerapp.push("${env.BUILD_ID}")
                    }
                }
            }
        }

        //Aqui é a parte de Entrega Continua, CD.

        stage('Deploy Kubernetes'){
            steps{
                script{
                    withKubeConfig([credentialsID: 'kubeconfig']){
                        sh 'kubectl apply -f ./k8s/deployment.yaml'
                    }
                }
            }
        }


    }
}