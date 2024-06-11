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
                        sh 'sudo echo ${{ secrets.DOCKER_PASSWORD }} | docker login --username bayshihin --password-stdin'
                        sh 'sudo docker push bayhtml'
                    }
                }
            }
        }
        stage('Deploy to Second Server') {
            steps {
                script {
                    // Остановка текущего компоуза и запуск нового с обновленным имеджем
                    sshCommand remote: '46.229.213.138', command: 'docker-compose down'
                    sshCommand remote: '46.229.213.138', command: 'docker-compose up -d'
                }
            }
        }
    }
}
