pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/SatishThapak/Argocd_demo.git', branch: 'main'
            }
        }
        stage('Build') {
            steps {
                sh 'echo "Building project..."'
                // Add build commands here, e.g., mvn clean install or docker build
            }
        }
        stage('Test') {
            steps {
                sh 'echo "Running tests..."'
                // Add test commands here
            }
        }
        stage('Deploy to ArgoCD') {
            steps {
                sh 'echo "Triggering ArgoCD sync..."'
                // Example: use argocd CLI to sync
                // sh 'argocd app sync argocd-demo-app'
            }
        }
    }
}
