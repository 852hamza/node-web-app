pipeline {
    agent any

    environment {
        AWS_REGION = 'eu-north-1'
        AWS_ACCOUNT_ID = '977098996049'
        REPOSITORY_URI = "${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/node-web"
        IMAGE_NAME = 'my-node-app'
        EC2_INSTANCE_IP = '13.48.10.87'
    }

    stages {
        stage('Checkout Code') {
            steps {
                // Checkout the code from your GitHub repository
                git 'https://github.com/852hamza/node-web-app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image using the Docker plugin
                    def app = docker.build(IMAGE_NAME, './node-js')
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
                    docker.withRegistry('https://${REPOSITORY_URI}', 'ecr:aws') {
                        app.push('latest')
                    }
                }
            }
        }

        stage('Deploy to EC2') {
            steps {
                script {
                    // Use the credentials stored in Jenkins
                    withCredentials([sshUserPrivateKey(credentialsId: 'node-web', keyVariable: 'SSH_KEY')]) {
                        // SSH into the EC2 instance and deploy the Docker container
                        sh """
                        ssh -o StrictHostKeyChecking=no -i ${SSH_KEY} ubuntu@${EC2_INSTANCE_IP} << 'EOF'
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
    }

    post {
        always {
            // Clean up Docker images after the build
            sh "docker rmi ${IMAGE_NAME} || true"
            sh "docker rmi ${REPOSITORY_URI}:latest || true"
        }
    }
}
