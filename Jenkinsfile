pipeline {
    agent any
    environment {
        FRONTEND_IMAGE_NAME = "practical-devops-frontend"
        BACKEND_IMAGE_NAME = "practical-devops-backend"
        ECR_URI = "145048992629.dkr.ecr.us-east-1.amazonaws.com/sd4413-practical-devops"
        FRONTEND_TAG = "frontend-latest"
        BACKEND_TAG = "backend-latest"
        REGION = "us-east-1"
        EKS_CLUSTER_NAME = "sd4413-eks"
    }

    stages {
        stage('Login to ECR') {
            steps {
                script {
                    echo "Logging in to ECR..."
                    sh "aws ecr get-login-password --region ${REGION} | docker login --username AWS --password-stdin ${ECR_URI}"
                }
            }
        }

        stage('Build Front End Image') {
            steps {
                echo "Building Frontend Image..."
                sh "docker build -t ${FRONTEND_IMAGE_NAME}:latest -f Dockerfile src/frontend"
                sh "docker tag ${FRONTEND_IMAGE_NAME}:latest ${ECR_URI}:${FRONTEND_TAG}"
                sh "docker push ${ECR_URI}:${FRONTEND_TAG}"
            }
        }

        stage('Build Back End Image') {
            steps {
                echo "Building Backend Image..."
                sh "docker build -t ${BACKEND_IMAGE_NAME}:latest -f Dockerfile src/backend"
                sh "docker tag ${BACKEND_IMAGE_NAME}:latest ${ECR_URI}:${BACKEND_TAG}"
                sh "docker push ${ECR_URI}:${BACKEND_TAG}"
            }
        }

        stage('Deploy k8s') {
            steps {
                echo "Deploying to Kubernetes..."
                sh "aws eks update-kubeconfig --name ${EKS_CLUSTER_NAME}"
                sh "kubectl apply -f k8s/aws/mongodb.yaml"
                sh "kubectl apply -f k8s/aws/backend.yaml"
                sh "kubectl apply -f k8s/aws/frontend.yaml"
            }
        }
    }
}
