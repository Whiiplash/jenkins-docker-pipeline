pipeline {
    agent any

    stages {
        stage('Clonar repositorio') {
            steps {
                git branch: 'main', url: 'https://github.com/Whiiplash/jenkins-docker-pipeline.git'
            }
        }

        stage('Construir imagen Docker') {
            steps {
                script {
                    dockerImage = docker.build("flask-app")
                }
            }
        }

        stage('Correr contenedor') {
            steps {
                script {
                    dockerImage.run("-d -p 5000:5000 flask-app")
                }
            }
        }
    }
}
