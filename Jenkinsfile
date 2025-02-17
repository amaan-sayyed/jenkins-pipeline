pipeline {
    agent any

    stages {
        stage("Code clone") {
            steps {
                echo "Cloning the code"
                git url: "https://github.com/amaan-sayyed/jenkins-pipeline.git", branch: 'main'
            }
        }
        stage("Code Build") {
            steps {
                echo "Building the image"
                sh 'docker-compose --verbose build'
            }
        }
        stage("Push to DockerHub") {
            steps {
                echo "Pushing the image to Docker Hub"
                withCredentials([usernamePassword(credentialsId: "dockerHub", passwordVariable: "dockerHubPass", usernameVariable: "dockerHubUser")]) {
                    sh "docker tag mern-test_frontend ${env.dockerHubUser}/mern-cicd:latest"
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                    sh "docker push ${env.dockerHubUser}/mern-cicd:latest"
                }
            }
        }
        stage("Deploy to EC2") {
            steps {
                echo "Deploying to EC2"
                withCredentials([sshUserPrivateKey(credentialsId: 'ec2-ssh-key', keyFileVariable: 'SSH_KEY')]) {
                    sh """
                        scp -i $SSH_KEY docker-compose.yaml ec2-user@54.157.194.36:/home/ec2-user/
                        scp -i $SSH_KEY startup-script.sh ec2-user@54.157.194.36:/home/ec2-user/
                        ssh -i $SSH_KEY ec2-user@54.157.194.36 'bash /home/ec2-user/startup-script.sh'
                    """
                }
            }
        }
    }
}
