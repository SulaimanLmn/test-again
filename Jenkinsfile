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
                    // Force a fresh build to include all updates
                    echo 'Building Docker image without caching...'
                    bat 'docker build --no-cache -t project-4-image .'
                }
            }
        }
        stage('Run Docker Container') {
            steps {
                script {
                    echo 'Stopping and removing any existing container...'
                    bat 'docker stop project-4-container || exit 0'
                    bat 'docker rm project-4-container || exit 0'

                    echo 'Running new container...'
                    bat 'docker run --rm -d --name project-4-container -p 8082:80 project-4-image'
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
