pipeline {
    agent any
    
    stages {
        stage('Build') {
            steps {
                // Build Backend
                sh '''
                    echo "Building Backend..."
                    cd blog-backend
                    npm install
                    npm run build
                '''
                
                // Build Frontend
                sh '''
                    echo "Building Frontend..."
                    cd blog-frontend
                    npm install
                    npm run build
                '''
            }
        }
    }
}
