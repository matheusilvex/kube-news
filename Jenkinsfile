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
            environment{
                tag_version = "${env.BUILD_ID}"
            }
            steps{
                script{
                    withKubeConfig([credentialsId: 'kubeconfig']){
                        sh 'sed -i "s/{{TAG}}/$tag_version/g" ./k8s/deployment.yaml'
                        sh 'kubectl apply -f ./k8s/deployment.yaml'
                    }
                }
            }
        }


    }
}