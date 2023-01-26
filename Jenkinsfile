pipeline {
    
    agent any

    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    dockerapp = docker.build("rodrigoandrade08/kube-news:${env.BUILD_ID}", '-f ./src/Dockerfile ./src')
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('http://registry.hub.docker.com', 'DOCKER_HUB'){
                        dockerapp.push('latest')
                        dockerapp.push("${env.BUILD_ID}")
                    }
                }
            }
        }
        stage('Deploy Kubernetes') {
            steps {
                withKubeConfig([credentialsId: 'KUBE_CONFIG']){
                    sh 'kubectl apply -f ./k8s/deployment.yaml'
                }        
            }
        }        
    }

}