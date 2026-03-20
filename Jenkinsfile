pipeline {
    agent any

    environment {
        IMAGE_NAME = 'shrikant155/html-app'
        DOCKERHUB_CREDENTIALS = 'dockerhub-cred-id'
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/Shrikant155/docker-project.git'
            }
        }

        stage('Build Image') {
            steps {
                sh 'docker build --no-cache -t $IMAGE_NAME:latest .'
            }
        }

        stage('Run Container (Test)') {
            steps {
                sh '''
                docker rm -f html-container || true
                docker run -d -p 8081:80 --name html-container $IMAGE_NAME:latest
                '''
            }
        }

        stage('Docker Login') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: "${DOCKERHUB_CREDENTIALS}",
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                }
            }
        }

        stage('Push Image') {
            steps {
                sh 'docker push $IMAGE_NAME:latest'
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                docker rm -f html-container || true
                docker pull $IMAGE_NAME:latest
                docker run -d -p 8081:80 --name html-container $IMAGE_NAME:latest
                '''
            }
        }
    }
}
