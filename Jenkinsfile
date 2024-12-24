pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                // Clone the public GitHub repository with the specified branch
                git branch: 'branch-name', url: 'https://github.com/SulaimanLmn/project-4-devops.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    echo 'Building Docker image...'
                    // Build the Docker image using the Dockerfile from the cloned repository
                    sh 'docker build -t project-4-image .'
                }
            }
        }
        stage('Run Docker Container') {
            steps {
                script {
                    echo 'Stopping and removing any existing container...'
                    // Stop and remove any existing container with the same name
                    sh 'docker stop project-4-container || true'
                    sh 'docker rm project-4-container || true'

                    echo 'Running new container...'
                    // Run a new container from the built image
                    sh 'docker run --rm -d --name project-4-container -p 8081:80 project-4-image'
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline executed successfully!'
        }
        always {
            echo 'Pipeline completed!'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}
