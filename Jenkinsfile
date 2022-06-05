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
    }

}