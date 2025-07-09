pipeline {
    agent any

    environment {
        IMAGE_NAME = "flask-app"
    }

    stages {
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
                        docker ps | grep flask-container || true
                    '''
                }
            }
        }

        // 	Task 4c: Etiquetar imagen con SHA del commit
        stage('Taggear imagen con SHA') {
            steps {
                script {
                    sh '''
                        COMMIT_SHA=$(git rev-parse --short HEAD)
                        echo "Tag del commit: $COMMIT_SHA"
                        docker tag $IMAGE_NAME whiiplash/$IMAGE_NAME:$COMMIT_SHA
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
