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
        stage("Deploy") {
            steps {
                echo "Deploying the container"
                sh "docker-compose up -d"
            }
        }
    }
}
