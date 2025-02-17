# MERN Stack CI/CD Pipeline Project

This repository demonstrates setting up an end-to-end CI/CD pipeline for a MERN (MongoDB, Express.js, React.js, Node.js) stack application.

## Overview

This project showcases a comprehensive CI/CD pipeline that automates the build and deployment processes for a 3-tier MERN stack application.
I have created a Jenkins pipeline which when triggered, increments the application version for the frontend and backend service, builds and pushes docker images to DockerHub with dynamnic version tags which were incremented in the previous stage, commits back the version bump to the repository and deploys the application on an EC2 Instance which acts like a deployment server using a shell script and Docker-Compose. 

For detailed information and execution steps, please refer to my [blog post](https://amaansayyed.hashnode.dev/seamless-mern-stack-deployment-with-jenkins-cicd-automation) where I provide step-by-step instructions and insights into each stage of the pipeline setup.

Checkout the same pipeline setup using [GitHub-Actions](https://github.com/amaan-sayyed/MERN-Action-Workflows)
