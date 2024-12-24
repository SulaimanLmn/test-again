pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/SulaimanLmn/project-4-devops.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    echo 'Building Docker image...'
                    // Use Windows Batch command to build the Docker image
                    bat 'docker build -t project-4-image .'
                }
            }
        }
        stage('Run Docker Container') {
            steps {
                script {
                    echo 'Stopping and removing any existing container...'
                    // Stop and remove any existing container with the same name
                    bat 'docker stop project-4-container || exit 0'
                    bat 'docker rm project-4-container || exit 0'

                    echo 'Running new container...'
                    // Run a new container from the built image
                    bat 'docker run --rm -d --name project-4-container -p 8081:80 project-4-image'
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
