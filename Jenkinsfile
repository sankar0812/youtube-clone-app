pipeline {
    agent any
    stages {
        stage('Checkout from Git') {
            steps {
                git branch: 'main', url: 'https://github.com/sankar0812/youtube-clone-app.git'
            }
        }
        stage('Install Dependencies') {
            steps {
                sh "npm install"
            }
        }
        stage("Docker Build & Push"){
             steps{
                 script{
                   withDockerRegistry(credentialsId: 'dockerhub', toolName: 'docker'){   
                      sh "docker build -t youtube-clone ."
                      sh "docker tag youtube-clone ashfaque9x/youtube-clone:latest "
                      sh "docker push ashfaque9x/youtube-clone:latest "
                    }
                }
            }
        }
        stage('Deploy to Kubernets'){
            steps{
                script{
                    dir('Kubernetes') {
                      withKubeConfig(caCertificate: '', clusterName: '', contextName: '', credentialsId: 'kubernetes', namespace: '', restrictKubeConfigAccess: false, serverUrl: '') {
                      sh 'kubectl delete --all pods'
                      sh 'kubectl apply -f deployment.yml'
                      sh 'kubectl apply -f service.yml'
                      }   
                    }
                }
            }
        }
    }
}
