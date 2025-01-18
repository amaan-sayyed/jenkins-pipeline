pipeline {
    agent any

    environment {
        DOCKER_REGISTRY = 'amaan72211'
        DOCKER_IMAGE_BACKEND = 'mern-backend'
        DOCKER_IMAGE_FRONTEND = 'mern-frontend'
        REGISTRY_CREDENTIALS = 'docker-credentials-id'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/your-repo.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh '''
                    cd backend && npm install
                    cd ../frontend && npm install
                '''
            }
        }

        stage('Run Tests') {
            steps {
                sh '''
                    cd backend && npm test
                    cd ../frontend && npm test
                '''
            }
        }

        stage('Build Docker Images') {
            steps {
                sh 'docker-compose build'
            }
        }

        stage('Push Docker Images') {
            steps {
                withCredentials([usernamePassword(credentialsId: env.REGISTRY_CREDENTIALS, usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh '''
                        echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                        docker tag mern-backend:latest $DOCKER_REGISTRY/$DOCKER_IMAGE_BACKEND:latest
                        docker tag mern-frontend:latest $DOCKER_REGISTRY/$DOCKER_IMAGE_FRONTEND:latest
                        docker push $DOCKER_REGISTRY/$DOCKER_IMAGE_BACKEND:latest
                        docker push $DOCKER_REGISTRY/$DOCKER_IMAGE_FRONTEND:latest
                    '''
                }
            }
        }

        stage('Deploy') {
            steps {
                sh 'docker-compose down && docker-compose up -d'
            }
        }
    }

    post {
        always {
            echo 'Pipeline completed.'
        }
    }
}
