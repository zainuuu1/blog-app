pipeline {
    agent any
    
    stages {
        stage('Setup') {
            steps {
                sh '''
                    # Install Node.js if not present
                    if ! command -v npm &> /dev/null; then
                        echo "Installing Node.js..."
                        curl -fsSL https://deb.nodesource.com/setup_16.x | sudo -E bash -
                        sudo apt-get install -y nodejs
                    fi
                    
                    # Verify
                    /usr/bin/node --version
                    /usr/bin/npm --version
                '''
            }
        }
        
        stage('Build') {
            steps {
                sh '''
                    # Use absolute paths
                    /usr/bin/npm --version
                    
                    # Backend
                    cd blog-backend
                    /usr/bin/npm install
                    /usr/bin/npm run build
                    
                    # Frontend
                    cd ../blog-frontend
                    /usr/bin/npm install
                    /usr/bin/npm run build
                '''
            }
        }
    }
}
