pipeline {
    agent any

    stages {
        stage('Git Checkout') {
            steps {
                // Checkout code from the Git repository
                git branch: 'main', changelog: false, poll: false, url: 'https://github.com/marcosvbras/todo-list-python.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    // Authenticate with Docker Hub using Jenkins credentials
                    withCredentials([usernamePassword(credentialsId: 'd4f525b1-568d-4c2e-8948-e6dec7087cb3', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        sh 'docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD'
                    }
                    
                    // Build Docker image
                    sh 'docker build -t hibajameel/sampleapp:latest .'
                }
            }
        }
        stage('Push to Docker Hub') {
            steps {
                // Push the Docker image to Docker Hub
                sh 'docker push hibajameel/sampleapp:latest'
            }
        }
        stage('Deploy the Container') {
            steps {
                // Pull the latest Docker image
                sh 'docker pull hibajameel/sampleapp:latest'
                
                // Stop and remove the existing container if it exists
                sh 'docker stop sampleapp || true'
                sh 'docker rm sampleapp || true'
                
                // Run the Docker container
                sh 'docker run -dp 80:80 --name sampleapp hibajameel/sampleapp:latest'
            }
        }
    }
}
