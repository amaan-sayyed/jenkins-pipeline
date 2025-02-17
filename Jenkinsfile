pipeline {
    agent any

    stages {
        stage("Clone Repository") {
            steps {
                echo "Cloning the repository"
                git url: 'https://github.com/amaan-sayyed/jenkins-pipeline.git', branch: 'main'
            }
        }
        stage("Build and Push Backend Image") {
            steps {
                echo "Building and pushing the backend image"
                withCredentials([usernamePassword(credentialsId: "dockerHub", passwordVariable: "DOCKER_HUB_PASS", usernameVariable: "DOCKER_HUB_USER")]) {
                    dir('./mern/backend'){
                    sh 'docker build -t ${DOCKER_HUB_USER}/mern-backend:latest .'
                    sh 'docker login -u ${DOCKER_HUB_USER} -p ${DOCKER_HUB_PASS}'
                    sh 'docker push ${DOCKER_HUB_USER}/mern-backend:latest'
                    }
                }
            }
        }
        stage("Build and Push Frontend Image") {
            steps {
                echo "Building and pushing the frontend image"
                withCredentials([usernamePassword(credentialsId: "dockerHub", passwordVariable: "DOCKER_HUB_PASS", usernameVariable: "DOCKER_HUB_USER")]) {
                    dir('./mern/frontend'){
                    sh 'docker build -t ${DOCKER_HUB_USER}/mern-frontend:latest .'
                    sh 'docker login -u ${DOCKER_HUB_USER} -p ${DOCKER_HUB_PASS}'
                    sh 'docker push ${DOCKER_HUB_USER}/mern-frontend:latest'
                    }
                }
            }
        }
        stage("Deploy to EC2") {
            steps {
                echo "Deploying to EC2"
                withCredentials([sshUserPrivateKey(credentialsId: 'ec2-ssh-key', keyFileVariable: 'SSH_KEY')]) {
                    sh """
                        mkdir -p ~/.ssh
                        ssh-keyscan -H 54.157.194.36 >> ~/.ssh/known_hosts
                        scp -i $SSH_KEY docker-compose.yaml ubuntu@54.157.194.36:/home/ubuntu/
                        scp -i $SSH_KEY startup-script.sh ubuntu@54.157.194.36:/home/ubuntu/
                        ssh -i $SSH_KEY ubuntu@54.157.194.36 'bash /home/ubuntu/startup-script.sh'
                    """
                }
            }
        }
    }
}