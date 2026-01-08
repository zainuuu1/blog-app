pipeline {
    agent any
    
    stages {
        stage('Build') {
            steps {
                sh '''
                    echo "=== Building with Node running npm ==="
                    
                    # Use node to execute npm
                    NODE="/usr/bin/node"
                    NPM_SCRIPT="/usr/lib/node_modules/npm/bin/npm-cli.js"
                    
                    echo "Node version: $($NODE --version)"
                    echo "npm script exists: $(ls $NPM_SCRIPT)"
                    
                    # Build Backend
                    echo "1. Building Backend..."
                    cd blog-backend
                    $NODE $NPM_SCRIPT install
                    $NODE $NPM_SCRIPT run build
                    
                    # Build Frontend
                    echo "2. Building Frontend..."
                    cd ../blog-frontend
                    $NODE $NPM_SCRIPT install
                    $NODE $NPM_SCRIPT run build
                    
                    echo "✅ Build Complete!"
                '''
            }
        }
    }
}
