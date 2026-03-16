pipeline {
    agent any

    stages {

        stage('Clone Repository') {
            steps {
                git 'https://github.com/sreepathysois/node-hello_Lab_Exam_Batch_2.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Run Application Locally') {
            steps {
                sh 'node index.js &'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t node-hello .'
            }
        }

        stage('Load Image to Minikube') {
            steps {
                sh 'minikube image load node-hello'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f deployment.yaml'
                sh 'kubectl apply -f service.yaml'
            }
        }

    }
}