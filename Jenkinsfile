pipeline {
    agent any

    environment {
        IMAGE_NAME = "flask-app"
        CONTAINER_NAME = "flask-container"
        HOST_PORT = "5001"
        CONTAINER_PORT = "5000"
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
                    sh 'docker rm -f $CONTAINER_NAME || true'
                    sh 'docker run -d -p $HOST_PORT:$CONTAINER_PORT --name $CONTAINER_NAME $IMAGE_NAME'
                    sleep(time: 5, unit: "SECONDS")
                    sh 'docker ps | grep $CONTAINER_NAME'
                }
            }
        }

        stage('Finalizar contenedor') {
            steps {
                sh 'docker stop $CONTAINER_NAME || true'
                sh 'docker rm $CONTAINER_NAME || true'
            }
        }
    }
}