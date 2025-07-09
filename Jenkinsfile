pipeline {
    agent any

    environment {
        IMAGE_NAME = "flask-app"
    }

    stages {
        stage('Clonar repositorio') {
            steps {
                git branch: 'main', url: 'https://github.com/Whiiplash/jenkins-docker-pipeline.git'
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
                    // Elimina si ya existe un contenedor con ese nombre
                    sh 'docker stop flask-container || true'
                    sh 'docker rm flask-container || true'

                    // Corre el contenedor
                    sh 'docker run -d -p 5000:5000 --name flask-container $IMAGE_NAME'

                    // Espera y muestra estado
                    sleep(time: 5, unit: "SECONDS")
                    sh 'docker ps | grep flask-container || true'
                }
            }
        }

        stage('Finalizar contenedor') {
            steps {
                sh 'docker stop flask-container || true'
                sh 'docker rm flask-container || true'
            }
        }
    }
}
