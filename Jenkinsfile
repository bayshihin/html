pipeline {
    agent any
    
    stages {
        stage('Build') {
            steps {
                script {
                    git 'https://github.com/bayshihin/html'
                    sh 'sudo docker build -t bayshihin/bayhtml .'
                }
            }
        }
        stage('Push to Docker Hub') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'dockercredentials', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        sh 'sudo docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD'
                        sh 'sudo docker push bayshihin/bayhtml'
                    }
                }
            }
        }
        stage('Deploy on Remote Server') {
            steps {
                withCredentials([sshUsernamePrivateKey(credentialsId: 'sshremote', usernameVariable: 'SSH_USER', privateKeyVariable: 'SSH_KEY')]) {
                    sshAgent (credentials: ['sshremote']) {
                        sh "ssh -i $SSH_KEY $SSH_USER@46.229.213.138 'cd /root/compose && docker-compose down'"
                        sh "ssh -i $SSH_KEY $SSH_USER@46.229.213.138 'cd /root/compose && docker-compose up -d'"
                }
            }
        }
    }
}
