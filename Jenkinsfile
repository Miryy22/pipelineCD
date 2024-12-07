pipeline {
    agent any
    environment {
        DOCKER_HOST = 'tcp://production-server:2375'
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/mon-projet.git'
            }
        }
        stage('Build Docker Images') {
            steps {
                sh 'docker-compose build'
            }
        }
        stage('Push Docker Images') {
            steps {
                sh 'docker-compose push'
            }
        }
        stage('Deploy to Production') {
            steps {
                sshagent(['jenkins-ssh-key']) {
                    sh """
                    ssh user@production-server '
                    cd /chemin/vers/docker-compose &&
                    docker-compose pull &&
                    docker-compose up -d
                    '
                    """
                }
            }
        }
    }
    post {
        success {
            echo 'Déploiement réussi !'
        }
        failure {
            echo 'Échec du déploiement.'
        }
    }
}
