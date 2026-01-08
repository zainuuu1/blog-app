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
        
        stage('Deploy') {
            steps {
                sh '''
                    echo "=== Starting Deployment ==="
                    
                    # 1. Deploy Backend with PM2
                    echo "Deploying Backend..."
                    cd blog-backend
                    
                    # Install production dependencies only
                    npm install --production
                    
                    # Start/Restart with PM2
                    pm2 delete blog-backend 2>/dev/null || true
                    pm2 start dist/main.js --name "blog-backend"
                    pm2 save
                    
                    echo "✅ Backend deployed on port 5000"
                    
                    # 2. Deploy Frontend to Nginx
                    echo "Deploying Frontend..."
                    cd ../blog-frontend
                    
                    # Copy to web root
                    sudo cp -r build/* /var/www/html/
                    
                    # Reload nginx
                    sudo nginx -s reload
                    
                    echo "✅ Frontend deployed to /var/www/html/"
                    echo "🎉 Deployment Complete!"
                    echo "Frontend: http://$(curl -s ifconfig.me)"
                    echo "Backend API: http://$(curl -s ifconfig.me):5000"
                '''
            }
        }
    }
}
