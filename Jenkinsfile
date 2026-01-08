pipeline {
    agent any
    
    stages {
        stage('Build') {
            steps {
                sh '''
                    echo "=== Starting Build ==="
                    
                    # Backend
                    cd blog-backend
                    echo "Installing backend dependencies..."
                    node /usr/bin/npm install --ignore-scripts
                    
                    echo "Building backend with skipLibCheck..."
                    # Use tsc from node_modules/.bin
                    ./node_modules/.bin/tsc --skipLibCheck
                    
                    # Check if build succeeded
                    if [ ! -d "dist" ]; then
                        echo "ERROR: Backend build failed - no dist folder"
                        exit 1
                    fi
                    
                    echo "✅ Backend built successfully!"
                    
                    # Frontend
                    cd ../blog-frontend
                    echo "Installing frontend dependencies..."
                    node /usr/bin/npm install
                    
                    echo "Building frontend with OpenSSL fix..."
                    NODE_OPTIONS="--openssl-legacy-provider" node /usr/bin/npm run build
                    
                    # Check if build succeeded
                    if [ ! -d "build" ]; then
                        echo "ERROR: Frontend build failed - no build folder"
                        exit 1
                    fi
                    
                    echo "✅ Frontend built successfully!"
                    echo "🎉 All builds completed!"
                '''
            }
        }
    }
}
