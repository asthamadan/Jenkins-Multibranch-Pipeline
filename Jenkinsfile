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
        DEPLOY_SERVER = 'your-deploy-server' // Replace with your deployment server IP/hostname
    }
    stages {
        stage('Checkout') {
            steps {
                // Checkout code from the specific branch
                git branch: "${env.BRANCH_NAME}", credentialsId: 'github-credentials-id', url: 'https://github.com/your-repo/your-app.git'
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
                sh 'docker ishte run --rm ${DOCKER_REGISTRY}/${IMAGE_NAME} npm test' // For Node.js app
                
                            }
        }
        stage('Push to Registry') {
            when {
                // Push only for main or develop branches
                anyOf {
                    branch 'main'
                    branch 'develop'
                }
            }
            steps {
                // Login to Docker registry (e.g., Docker Hub)
                sh 'echo $DOCKER_CREDENTIALS_PSW | docker login ${DOCKER_REGISTRY} --username $DOCKER_CREDENTIALS_USR --password-stdin'
                // Push Docker image
                sh 'docker push ${DOCKER_REGISTRY}/${IMAGE_NAME}'
            }
        }
        stage('Deploy') {
            when {
                // Deploy only for main or develop branches
                anyOf {
                    branch 'main'
                    branch 'develop'
                }
            }
            steps {
                script {
                    if (env.BRANCH_NAME == 'main') {
                        // Deploy to production server (example: SSH to server and run Docker container)
                        sh '''
                            ssh -o StrictHostKeyChecking=no user@${DEPLOY_SERVER} << 'EOF'
                            docker pull ${DOCKER_REGISTRY}/${IMAGE_NAME}
                            docker stop myapp-prod || true
                            docker rm myapp-prod || true
                            docker run -d --name myapp-prod -p 80:3000 ${DOCKER_REGISTRY}/${IMAGE_NAME}
                            EOF
                        '''
                    } else if (env.BRANCH_NAME == 'develop') {
                        // Deploy to staging server
                        sh '''
                            ssh -o StrictHostKeyChecking=no user@${DEPLOY_SERVER} << 'EOF'
                            docker pull ${DOCKER_REGISTRY}/${IMAGE_NAME}
                            docker stop myapp-staging || true
                            docker rm myapp-staging || true
                            docker run -d --name myapp-staging -p 8080:3000 ${DOCKER_REGISTRY}/${IMAGE_NAME}
                            EOF
                        '''
                    }
                }
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
}

