pipeline {
    agent any
    
    stages {
        // Stage 1: Get Code
        stage('Checkout') {
            steps {
                git 'https://github.com/zainuuu1/blog-app'
            }
        }
        
        // Stage 2: Build Backend
        stage('Build Backend') {
            steps {
                dir('blog-backend') {
                    sh 'npm install'
                    sh 'npm run build'
                }
            }
        }
        
        // Stage 3: Build Frontend
        stage('Build Frontend') {
            steps {
                dir('blog-frontend') {
                    sh 'npm install'
                    sh 'npm run build'
                }
            }
        }
    }
    
    post {
        always {
            echo 'Build process completed!'
        }
    }
}
