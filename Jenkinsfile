pipeline {
    agent any

    environment {
        AWS_REGION = 'eu-north-1'
        AWS_ACCOUNT_ID = '977098996049'
        REPOSITORY_URI = "${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/node-web"
        IMAGE_NAME = 'my-node-app'
        EC2_INSTANCE_IP = '13.48.10.87'
        SSH_KEY_PATH = 'path/to/Node-web-automation.pem' // Adjust the path to your SSH key
        SSH_USER = 'ubuntu' // Adjust if needed
    }

    stages {
        stage('Checkout Code') {
            steps {
                // Checkout the code from your GitHub repository
                git ''
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image
                    sh "docker build -t ${IMAGE_NAME} ./node-js"
                    sh "docker tag ${IMAGE_NAME}:latest ${REPOSITORY_URI}:latest"
                }
            }
        }

        stage('Login to Amazon ECR') {
            steps {
                script {
                    // Log in to ECR
                    sh "aws ecr get-login-password --region ${AWS_REGION} | docker login --username AWS --password-stdin ${REPOSITORY_URI}"
                }
            }
        }

        stage('Push Docker Image to ECR') {
            steps {
                script {
                    // Push the Docker image to ECR
                    sh "docker push ${REPOSITORY_URI}:latest"
                }
            }
        }

        stage('Deploy to EC2') {
            steps {
                script {
                    // SSH into the EC2 instance and deploy the Docker container
                    sh """
                    ssh -o StrictHostKeyChecking=no -i ${SSH_KEY_PATH} ${SSH_USER}@${EC2_INSTANCE_IP} << 'EOF'
                        docker pull ${REPOSITORY_URI}:latest
                        docker stop ${IMAGE_NAME} || true
                        docker rm ${IMAGE_NAME} || true
                        docker run -d --name ${IMAGE_NAME} -p 3000:3000 ${REPOSITORY_URI}:latest
                    EOF
                    """
                }
            }
        }
    }

    post {
        always {
            // Clean up Docker images after the build
            sh "docker rmi ${IMAGE_NAME} || true"
            sh "docker rmi ${REPOSITORY_URI}:latest || true"
        }
    }
}
