pipeline{
    agent any
    
    stages{
        stage("Code clone"){
            steps{
                echo "CLoning the code"
                git url: "https://github.com/amaan-sayyed/jenkins-pipeline.git", branch: 'main'
            }
        }
        stage("Code Build"){
            steps{
                echo "Building the image"
                sh 'docker-compose build'
            }
        }
        stage("Push to DockerHub"){
            steps{
                echo "Pushing the image to docker hub"
            }
        }
        stage("Deploy"){
            steps{
                echo "Delpoying the container"
            }
        }
        
    }
}