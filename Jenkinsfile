pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                script {
                    echo "Cloning repository..."
                    // Clone the branch based on which one is being built
                    git branch: "${BRANCH_NAME}", url: 'https://github.com/SulaimanLmn/test-again.git'
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    echo "Building Docker image for ${BRANCH_NAME} branch..."
                    bat "docker build -t project-4-image-${BRANCH_NAME} ."
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    echo 'Stopping and removing any existing container...'
                    bat "docker stop project-4-container-${BRANCH_NAME} || exit 0"
                    bat "docker rm project-4-container-${BRANCH_NAME} || exit 0"

                    echo 'Running new container...'
                    bat "docker run --rm -d --name project-4-container-${BRANCH_NAME} -p 8081:80 project-4-image-${BRANCH_NAME}"
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
