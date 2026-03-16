pipeline {
    agent any

    environment {
        DOCKER_HUB = "pranaykumpala"
        IMAGE_NAME = "node-hello-devops"
        KUBECONFIG = "/var/snap/jenkins/5004/.kube/config"
    }

    stages {

        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/pranaybekal/node-hello-devops.git'
            }
        }

        stage('nodebaremetalcompile') {
            steps {
                echo "Installing Node Dependencies"
                sh 'npm install'
            }
        }

        stage('nodebaremetaltest') {
            steps {
                echo "Running Node Tests"
                sh 'npm test || true'
            }
        }

        stage('nodebaremetalpackagedeploy') {
            steps {
                echo "Packaging Node Application"
                sh 'tar -czf nodeapp.tar.gz *'
            }
        }

        stage('nodejs_docker') {
            steps {
                echo "Building Docker Image"
                sh 'docker build -t ${DOCKER_HUB}/${IMAGE_NAME}:latest .'
            }
        }

        stage('docker_push') {
            steps {
                echo "Pushing Docker Image"
                sh 'docker push ${DOCKER_HUB}/${IMAGE_NAME}:latest'
            }
        }

        stage('minikube_nodejs') {
            steps {
                echo "Checking Kubernetes Cluster"
                sh 'kubectl get nodes'

                echo "Deploying to Minikube"
                sh 'kubectl apply -f k8s/deployment.yaml --validate=false'
                sh 'kubectl apply -f k8s/service.yaml --validate=false'

                echo "Checking Pods"
                sh 'kubectl get pods'
            }
        }

    }
}