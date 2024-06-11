pipeline {
    agent any
    
    stages {
        stage('Build') {
            steps {
                script {
                    // Клонирование репозитория и сборка Docker-образа
                    git 'https://github.com/bayshihin/html'
                    sh 'sudo docker build -t bayhtml .'
                }
            }
        }
        stage('Push to Docker Hub') {
            steps {
                script {
                    // Пуш имеджа в Docker Hub
                    withCredentials([usernamePassword(credentialsId: 'dockercredentials', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        sh 'sudo docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD'
                        sh 'sudo docker push bayhtml'
                    }
                }
            }
        }
    }
}
