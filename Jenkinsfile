pipeline {
    agent {
        label 'Node1'
    }
    environment {
        DOCKER_REGISTRY = 'docker.io'
        IMAGE_NAME = "myapp:${env.BRANCH_NAME}-${env.BUILD_NUMBER}"
        DOCKER_CREDENTIALS = credentials('docker-credentials-id')
    }
    stages {
        stage('Checkout') {
            when {
                anyOf { branch 'main'; branch 'develop' }
            }
            steps {
                // Checkout is handled automatically by Jenkins Multibranch Pipeline,
                // but this stage can be used for additional SCM operations if needed
                echo 'Checking out code from repository'
                sh 'git rev-parse HEAD'
            }
        }
        stage('Build') {
            when {
                anyOf { branch 'main'; branch 'develop' }
            }
            steps {
                sh 'docker build --no-cache -t ${DOCKER_REGISTRY}/amadan783/${IMAGE_NAME} .'
            }
        }
        stage('Test') {
            when {
                anyOf { branch 'main'; branch 'develop' }
            }
            steps {
                sh 'docker run --rm ${DOCKER_REGISTRY}/amadan783/${IMAGE_NAME} npm test --passWithNoTests'
            }
        }
        stage('Deploy') {
            when {
                branch 'develop'
            }
            steps {
                // Simulate deployment by running the container locally on the agent
                sh 'docker run -d -p 3000:3000 ${DOCKER_REGISTRY}/amadan783/${IMAGE_NAME}'
                echo 'Application deployed on port 3000'
            }
        }
        stage('Push to Registry') {
            when {
                anyOf { branch 'main'; branch 'develop' }
            }
            steps {
                sh 'echo $DOCKER_CREDENTIALS_PSW | docker login ${DOCKER_REGISTRY} --username $DOCKER_CREDENTIALS_USR --password-stdin'
                sh 'docker push ${DOCKER_REGISTRY}/amadan783/${IMAGE_NAME}'
            }
        }
    }
    post {
        always {
            sh 'docker stop $(docker ps -q) || true' // Stop any running containers
            sh 'docker logout ${DOCKER_REGISTRY}'
        }
    }
}