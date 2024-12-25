pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                script {
                    echo "Cloning repository..."
                    // Use the correct variable for the branch name
                    git branch: "${env.BRANCH_NAME}", url: 'https://github.com/SulaimanLmn/test-again.git'
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    echo "Building Docker image for ${env.BRANCH_NAME} branch..."
                    bat "docker build -t project-4-image-${env.BRANCH_NAME} ."
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    // Define a predictable port for each branch
                    def branchPort
                    switch (env.BRANCH_NAME) {
                        case 'main':
                            branchPort = 8081
                            break
                        case 'development':
                            branchPort = 8082
                            break
                        default:
                            branchPort = 4000 // Default port for unknown branches
                    }

                    echo "Stopping and removing any existing container for ${env.BRANCH_NAME}..."
                    bat "docker stop project-4-container-${env.BRANCH_NAME} || exit 0"
                    bat "docker rm project-4-container-${env.BRANCH_NAME} || exit 0"

                    echo "Running new container for ${env.BRANCH_NAME} on port ${branchPort}..."
                    bat "docker run --rm -d --name project-4-container-${env.BRANCH_NAME} -p ${branchPort}:80 project-4-image-${env.BRANCH_NAME}"

                    echo "Container for ${env.BRANCH_NAME} is running on port ${branchPort}"
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
