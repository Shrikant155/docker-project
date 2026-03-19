pipeline {
    agent any

    environment {
        DOCKER_HUB = "yourdockerhubusername/my-html-app"
    }

    stages {

        stage('Clone') {
            steps {
                git '<your-github-repo-url>'
            }
        }

        stage('Build Image') {
            steps {
                sh 'docker build -t $DOCKER_HUB .'
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-cred',
                    usernameVariable: 'USER',
                    passwordVariable: 'PASS'
                )]) {
                    sh 'echo $PASS | docker login -u $USER --password-stdin'
                }
            }
        }

        stage('Push Image') {
            steps {
                sh 'docker push $DOCKER_HUB'
            }
        }
    }
}
