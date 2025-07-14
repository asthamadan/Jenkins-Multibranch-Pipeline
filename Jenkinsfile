pipeline {
    agent {
        node {
      label 'Node1'
    }
    }
    environment {
        // Define environment variables
        DOCKER_REGISTRY = 'docker.io/amadan783' // Replace with your Docker Hub username or registry
        DOCKER_CREDENTIALS = credentials('docker-credentials-id') // Jenkins credential ID for Docker Hub
        GIT_CREDENTIALS = credentials('github-credentials-id') // Jenkins credential ID for GitHub
        IMAGE_NAME = "myapp:${env.BRANCH_NAME}-${env.BUILD_NUMBER}"
        }
    stages {
        stage('Checkout') {
            steps {
                // Checkout code from the specific branch
                git branch: "${env.BRANCH_NAME}", credentialsId: 'github-credentials-id', url: 'https://github.com/asthamadan/Jenkins-Multibranch-Pipeline.git'
            }
        }
        stage('Build') {
            steps {
                // Build Docker image
                sh 'docker build -t ${DOCKER_REGISTRY}/${IMAGE_NAME} .'
            }
        }
        stage('Test') {
            when {
                // Run tests only for main or develop branches
                              
                    branch 'develop'
                }
            }
            steps {
                // Example: Run tests (modify based on your app's test framework)
                sh 'docker run --rm ${DOCKER_REGISTRY}/${IMAGE_NAME} npm test' // For Node.js app
                
                            }
        }
                
    }
    post {
        always {
            // Clean up Docker images to save space
            sh 'docker rmi ${DOCKER_REGISTRY}/${IMAGE_NAME} || true'
        }
        success {
            echo "Pipeline completed successfully for branch: ${env.BRANCH_NAME}"
        }
        failure {
            echo "Pipeline failed for branch: ${env.BRANCH_NAME}"
        }
    }


