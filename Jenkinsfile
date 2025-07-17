<<<<<<< HEAD
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

         steps {
                sh 'docker run --rm ${DOCKER_REGISTRY}/${IMAGE_NAME} npm test' 
                
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

<<<<<<< HEAD
=======

>>>>>>> 80e45305113440dcee71bdf7c62b53d63aaed26a
}
=======
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

         steps {
                sh 'docker run --rm ${DOCKER_REGISTRY}/${IMAGE_NAME} npm test' 
                
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

>>>>>>> 9798d627e65312990ea9659c6be8b5eb2d3b804b
