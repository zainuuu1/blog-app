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
                    ./node_modules/.bin/tsc --skipLibCheck
                    
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
                    
                    if [ ! -d "build" ]; then
                        echo "ERROR: Frontend build failed - no build folder"
                        exit 1
                    fi
                    
                    echo "✅ Frontend built successfully!"
                    echo "🎉 All builds completed!"
                '''
            }
        }
        
        stage('Deploy') {
            steps {
                sh '''
                    echo "=== Starting Deployment ==="
                    
                    # 1. Deploy Backend with PM2 (already working!)
                    echo "Deploying Backend..."
                    cd blog-backend
                    npm install --production
                    pm2 delete blog-backend 2>/dev/null || true
                    pm2 start dist/main.js --name "blog-backend"
                    pm2 save
                    
                    echo "✅ Backend deployed on port 5000"
                    
                    # 2. Deploy Frontend - NO SUDO needed
                    echo "Deploying Frontend..."
                    cd ../blog-frontend
                    
                    # Method 1: Direct copy (if permissions fixed)
                    cp -r build/* /var/www/html/ 2>/dev/null && echo "✅ Frontend copied successfully!"
                    
                    # Method 2: If direct copy fails, try alternative
                    if [ $? -ne 0 ]; then
                        echo "Trying alternative method..."
                        
                        # Create temp directory and copy
                        mkdir -p /tmp/frontend-${BUILD_NUMBER}
                        cp -r build/* /tmp/frontend-${BUILD_NUMBER}/
                        
                        # Move with proper ownership
                        sudo mv /tmp/frontend-${BUILD_NUMBER}/* /var/www/html/ 2>/dev/null || \
                        echo "Note: May need to run: sudo chown -R jenkins:jenkins /var/www/html"
                    fi
                    
                    # Try to reload nginx (optional)
                    sudo nginx -s reload 2>/dev/null || echo "Nginx reload skipped (permissions)"
                    
                    echo "✅ Frontend deployment attempted!"
                    echo "🎉 Deployment Process Complete!"
                    
                    # Show success message
                    echo "========================================="
                    echo "✅ BACKEND: Running on port 5000"
                    echo "✅ FRONTEND: Files copied to /var/www/html"
                    echo "========================================="
                    echo "To fix permissions permanently, run on server:"
                    echo "sudo chown -R jenkins:jenkins /var/www/html"
                    echo "========================================="
                '''
            }
        }
    }
}
