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
                    // Define a predictable port for each branch
                    def branchPort
                    switch (BRANCH_NAME) {
                        case 'main':
                            branchPort = 8081
                            break
                        case 'development':
                            branchPort = 8082
                            break
                       
                        default:
                            branchPort = 4000  // Default port for unknown branches
                    }

                    echo "Stopping and removing any existing container for ${BRANCH_NAME}..."
                    bat "docker stop project-4-container-${BRANCH_NAME} || exit 0"
                    bat "docker rm project-4-container-${BRANCH_NAME} || exit 0"

                    echo "Running new container for ${BRANCH_NAME} on port ${branchPort}..."
                    bat "docker run --rm -d --name project-4-container-${BRANCH_NAME} -p ${branchPort}:80 project-4-image-${BRANCH_NAME}"

                    echo "Container for ${BRANCH_NAME} is running on port ${branchPort}"
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
