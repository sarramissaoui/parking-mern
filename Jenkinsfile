pipeline {
    agent any

    environment {
  
        NODE_VERSION = '18'
        DOCKER_IMAGE = 'parking'
        BACKEND_DIR = 'backend'
        FRONTEND_DIR = 'frontend'
    }

    stages {
        stage('Checkout') {
            steps {
                // Clone the repository
                git branch: 'master', url: 'https://github.com/sarramissaoui/parking-mern.git
'
            }
        }

        stage('Install Dependencies (Backend)') {
            steps {
                dir("${BACKEND_DIR}") {
                    script {
                        
                        sh 'npm install'
                    }
                }
            }
        }

        stage('Install Dependencies (Frontend)') {
            steps {
                dir("${FRONTEND_DIR}") {
                    script {
                        // Install frontend dependencies
                        sh 'npm install'
                    }
                }
            }
        }

        stage('Build Backend') {
            steps {
                dir("${BACKEND_DIR}") {
                    script {
                        // Build the backend
                        sh 'npm run build'
                    }
                }
            }
        }

        stage('Build Frontend') {
            steps {
                dir("${FRONTEND_DIR}") {
                    script {
                        // Build the frontend
                        sh 'npm run build'
                    }
                }
            }
        }

        stage('Run Unit Tests') {
            steps {
                script {
                    // Run tests for both backend and frontend
                    sh 'npm run test'
                }
            }
        }

        stage('Dockerize Application') {
            steps {
                script {
                    // Build Docker image for the backend and frontend
                    sh '''
                        docker build -t ${DOCKER_IMAGE}-backend ./backend
                        docker build -t ${DOCKER_IMAGE}-frontend ./frontend
                    '''
                }
            }
        }

        stage('Push Docker Image to Registry') {
            steps {
                script {
                    // Push the Docker images to a Docker registry (e.g., DockerHub)
                    sh '''
                        docker tag ${DOCKER_IMAGE}-backend your-docker-repo/${DOCKER_IMAGE}-backend:latest
                        docker tag ${DOCKER_IMAGE}-frontend your-docker-repo/${DOCKER_IMAGE}-frontend:latest
                        docker push your-docker-repo/${DOCKER_IMAGE}-backend:latest
                        docker push your-docker-repo/${DOCKER_IMAGE}-frontend:latest
                    '''
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    // Deploy the Docker containers to a Kubernetes cluster
                    sh '''
                        kubectl apply -f k8s/backend-deployment.yaml
                        kubectl apply -f k8s/frontend-deployment.yaml
                    '''
                }
            }
        }
    }

    post {
        success {
            echo 'Build and Deployment Successful!'
        }
        failure {
            echo 'Build or Deployment Failed!'
        }
    }
}
