pipeline {
    agent any

    stages {
        stage('Verificar conexão Git') {
            steps {
                git url:'https://github.com/ediplomaia/EasyOps.git', branch:'dev'
            }
        }
    

    
        stage('Buil Imagem') {
            steps {
                script{
                    dockerapp = docker.build("ediplo/easyops:${env.BUILD_ID}",
                      '-f ./Dockerfile .')
                }
            }
        }
    
    
        stage('Push Imagem') {
            steps {
                script{
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                    dockerapp.push('latest')
                    dockerapp.push("${env.BUILD_ID}")

                    }
                }
            }
        }
        
        stage('Deploy Kubernetes') {
            agent {
                kubernetes {
                    cloud 'kubernetes'
                }
            }
            environment {
                tag_version = "${env.BUILD_ID}"
            }
            steps {
                script{
                    sh 'sed -i "s/{{tag}}/$tag_version/g" ./k8s/deployment.yaml'
                    sh 'cat ./k8s/deployment.yaml'
                    kubernetesDeploy(configs: '**/k8s/**', kubeconfigId: 'kubeconfig')
                }
            }
        }
    }
}