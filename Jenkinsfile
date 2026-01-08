pipeline {
    agent any
    
    stages {
        stage('Build') {
            steps {
                sh '''
                    echo "=== Building Application ==="
                    
                    # Backend
                    cd blog-backend
                    node /usr/bin/npm install --ignore-scripts
                    
                    # Remove conflicting Jasmine types (since project uses Jest)
                    echo "Removing @types/jasmine to fix TypeScript conflict..."
                    node /usr/bin/npm uninstall @types/jasmine --save-dev
                    
                    # Build
                    node /usr/bin/npm run build
                    
                    # Frontend
                    cd ../blog-frontend
                    node /usr/bin/npm install
                    node /usr/bin/npm run build
                    
                    echo "✅ Build Complete!"
                '''
            }
        }
    }
}
