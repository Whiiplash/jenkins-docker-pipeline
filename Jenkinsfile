pipeline {
    agent any

    environment {
        IMAGE_NAME = "flask-app"
    }

    stages {
        stage('Clonar repositorio') {
            steps {
                git 'https://github.com/Whiiplash/jenkins-docker-pipeline.git'
            }
        }

        stage('Construir imagen Docker') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Correr contenedor') {
            steps {
                script {
                    sh '''
                        docker rm -f flask-container || true
                        docker run -d -p 5001:5000 --name flask-container $IMAGE_NAME
                        sleep 5
                        docker ps | grep flask-container
                    '''
                }
            }
        }

        stage('Finalizar contenedor') {
            steps {
                sh '''
                    docker stop flask-container || true
                    docker rm flask-container || true
                '''
            }
        }
    }
}
