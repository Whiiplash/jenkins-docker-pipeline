pipeline {
    agent any

    environment {
        IMAGE_NAME = 'whiiplash/flask-app'
    }

    stages {
        stage('Construir imagen Docker') {
            steps {
                sh 'docker build -t flask-app .'
            }
        }

        stage('Correr contenedor') {
            steps {
                script {
                    sh '''
                        docker rm -f flask-container || true
                        docker run -d -p 5001:5000 --name flask-container flask-app
                        sleep 5
                        docker ps | grep flask-container || true
                    '''
                }
            }
        }

        stage('Taggear imagen con SHA') {
            steps {
                script {
                    sh '''
                        COMMIT_SHA=$(git rev-parse --short HEAD)
                        echo "Tag del commit: $COMMIT_SHA"
                        docker tag flask-app ${IMAGE_NAME}:$COMMIT_SHA
                    '''
                }
            }
        }

        stage('Push a Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    script {
                        sh '''
                            COMMIT_SHA=$(git rev-parse --short HEAD)
                            echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                            docker push ${IMAGE_NAME}:$COMMIT_SHA
                            docker logout
                        '''
                    }
                }
            }
        }

        stage('Finalizar contenedor') {
            steps {
                script {
                    sh '''
                        docker stop flask-container || true
                        docker rm flask-container || true
                    '''
                }
            }
        }
    }
}
