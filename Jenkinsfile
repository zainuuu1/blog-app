pipeline {
    agent any
    
    stages {
        stage('Build') {
            steps {
                sh '''
                    echo "=== Starting Build ==="
                    
                    # Test npm access
                    node /usr/bin/npm --version
                    
                    # Build Backend
                    echo "1. Building Backend..."
                    cd blog-backend
                    node /usr/bin/npm install
                    node /usr/bin/npm run build
                    
                    # Build Frontend
                    echo "2. Building Frontend..."
                    cd ../blog-frontend
                    node /usr/bin/npm install
                    node /usr/bin/npm run build
                    
                    echo "✅ Build Complete!"
                '''
            }
        }
    }
}
