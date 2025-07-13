pipeline {
    agent any 
    
    stages { 
        stage('SCM Checkout') {
            steps {
                retry(3) {
                    git branch: 'main', url: 'https://github.com/shanulkshan/Docker-and-Jenkins-CI-CD-Pipeline.git'
                }
            }
        }
        stage('Build Docker Image') {
            steps {  
                bat 'docker build -t shanu99/nodeapp-cuban:%BUILD_NUMBER% .'
            }
        }
        stage('Login to Docker Hub') {
            steps {
                withCredentials([string(credentialsId: 'test-dockerhubpassword', variable: 'test-dockerhubpass')]) {
                    script {
                        bat "docker login -u shanu99 -p %test-dockerhubpass%"
                    }
                }
            }
        }
        stage('Push Image') {
            steps {
                bat 'docker push shanu99/nodeapp-cuban:%BUILD_NUMBER%'
            }
        }
    }
    post {
        always {
            bat 'docker logout'
        }
    }
}
