pipeline {
    agent any

    stages {
        stage("Build Backend Image") {
            steps {
                echo "Building the backend image"
                withCredentials([usernamePassword(credentialsId: "dockerHub", passwordVariable: "DOCKER_HUB_PASS", usernameVariable: "DOCKER_HUB_USER")]) {
                    sh 'docker build -t ${DOCKER_HUB_USER}/mern-backend:latest -f backend/Dockerfile .'
                }
            }
        }
        stage("Build Frontend Image") {
            steps {
                echo "Building the frontend image"
                withCredentials([usernamePassword(credentialsId: "dockerHub", passwordVariable: "DOCKER_HUB_PASS", usernameVariable: "DOCKER_HUB_USER")]) {
                    sh 'docker build -t ${DOCKER_HUB_USER}/mern-frontend:latest -f frontend/Dockerfile .'
                }
            }
        }
        stage("Push Backend Image to DockerHub") {
            steps {
                echo "Pushing the backend image to Docker Hub"
                withCredentials([usernamePassword(credentialsId: "dockerHub", passwordVariable: "DOCKER_HUB_PASS", usernameVariable: "DOCKER_HUB_USER")]) {
                    sh 'docker login -u ${DOCKER_HUB_USER} -p ${DOCKER_HUB_PASS}'
                    sh 'docker push ${DOCKER_HUB_USER}/mern-backend:latest'
                }
            }
        }
        stage("Push Frontend Image to DockerHub") {
            steps {
                echo "Pushing the frontend image to Docker Hub"
                withCredentials([usernamePassword(credentialsId: "dockerHub", passwordVariable: "DOCKER_HUB_PASS", usernameVariable: "DOCKER_HUB_USER")]) {
                    sh 'docker login -u ${DOCKER_HUB_USER} -p ${DOCKER_HUB_PASS}'
                    sh 'docker push ${DOCKER_HUB_USER}/mern-frontend:latest'
                }
            }
        }
        stage("Deploy to EC2") {
            steps {
                echo "Deploying to EC2"
                withCredentials([sshUserPrivateKey(credentialsId: 'ec2-ssh-key', keyFileVariable: 'SSH_KEY')]) {
                    sh """
                        ssh-keyscan -H 54.157.194.36 >> ~/.ssh/known_hosts
                        scp -i $SSH_KEY docker-compose.yaml ec2-user@54.157.194.36:/home/ec2-user/
                        scp -i $SSH_KEY startup-script.sh ec2-user@54.157.194.36:/home/ec2-user/
                        ssh -i $SSH_KEY ec2-user@54.157.194.36 'bash /home/ec2-user/startup-script.sh'
                    """
                }
            }
        }
    }
}