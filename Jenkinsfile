pipeline {
    agent any 
    
    stages { 
        stage('SCM Checkout') {
            steps {
                retry(3) {
                    git branch: 'main', url: 'https://github.com/MadushanMRS/CICD-Pipeline'
                }
            }
        }
        stage('Build Docker Image') {
            steps {  
                bat 'docker build -t madushanliber/nodeapp-cicd:%BUILD_NUMBER% .'
            }
        }
        stage('Login to Docker Hub') {
            steps {
                withCredentials([string(credentialsId: 'test-dockerpassword', variable: 'nodecicd-pass')]) {
                    script {
                        bat "docker login -u madushanliber -p %nodecicd-pass%"
                    }
                }
            }
        }
        stage('Push Image') {
            steps {
                bat 'docker push madushanliber/nodeapp-cicd:%BUILD_NUMBER%'
            }
        }
    }
    post {
        always {
            bat 'docker logout'
        }
    }
}
