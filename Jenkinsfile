pipeline {
    agent any
    
    stages {
        // Stage 1: Get Code
        stage('Get Code') {
            steps {
                git 'https://github.com/zainuuu1/blog-app'
            }
        }
        
        // Stage 2: Setup Backend
        stage('Setup Backend') {
            steps {
                sh '''
                    echo "Setting up backend..."
                    cd blog-backend
                    
                    # Install dependencies
                    npm install
                    
                    # Create .env file if not exists
                    if [ ! -f .env ]; then
                        cp .env.example .env
                        echo "Created .env file. Please update with actual values."
                    fi
                    
                    # Build backend
                    npm run build
                '''
            }
        }
        
        // Stage 3: Setup Frontend
        stage('Setup Frontend') {
            steps {
                sh '''
                    echo "Setting up frontend..."
                    cd blog-frontend
                    
                    # Install dependencies
                    npm install
                    
                    # Create .env file if not exists
                    if [ ! -f .env ]; then
                        cp .env.example .env
                        echo "Created .env file. Please update with actual values."
                    fi
                    
                    # Build frontend
                    npm run build
                '''
            }
        }
        
        // Stage 4: Start Services
        stage('Start Services') {
            steps {
                sh '''
                    echo "Starting services..."
                    
                    # Start Backend with PM2
                    cd /var/www/blog/nest/blog-backend
                    
                    # Stop if already running
                    pm2 delete blog-backend 2>/dev/null || true
                    
                    # Start backend
                    pm2 start dist/main.js --name "blog-backend"
                    
                    # Save PM2 process list
                    pm2 save
                    
                    # Copy frontend to Nginx directory
                    sudo cp -r /var/www/blog/nest/blog-frontend/build/* /var/www/html/
                    
                    # Restart Nginx
                    sudo systemctl restart nginx
                    
                    echo "All services started!"
                '''
            }
        }
    }
    
    post {
        success {
            echo '✅ Deployment Successful!'
            echo 'Backend: http://localhost:5000'
            echo 'Frontend: http://localhost'
        }
        failure {
            echo '❌ Deployment Failed!'
        }
    }
}
