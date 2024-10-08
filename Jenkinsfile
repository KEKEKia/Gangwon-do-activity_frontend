pipeline {
    agent any
    environment {
        DOCKER_IMAGE = 'frontend-app:latest'
        NODE_HOME = '/usr/bin'  // Node.js와 npm의 경로를 확인하고 수정
        PATH = "${NODE_HOME}:${env.PATH}"
    }
    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out code...'
                git branch: 'main', url: 'https://github.com/K-srand/Gangwon-do-activity_frontend.git'
            }
        }
        stage('Install Dependencies') {
            steps {
                echo 'Installing dependencies...'
                sh 'npm install'
            }
        }
        stage('Build') {
            steps {
                echo 'Building the project...'
                sh '''
                unset CI
                npm run build
                '''
            }
        }
        stage('Docker Build') {
            steps {
                echo 'Building Docker image...'
                script {
                    dockerImage = docker.build(env.DOCKER_IMAGE)
                    echo "Docker image built successfully: ${dockerImage.id}"
                }
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying the application...'
                script {
                    sh 'docker stop frontend-app || true'
                    sh 'docker rm frontend-app || true'
                    sh 'docker run -d -p 3030:80 --name frontend-app ' + env.DOCKER_IMAGE
                    echo "Docker container started successfully"
                }
            }
        }
    }
}
