pipeline {
    agent any
    
    stages {
        stage('Build') {
            steps {
                sh '''
                    echo "=== Building with environment fix ==="
                    
                    # Backend
                    cd blog-backend
                    node /usr/bin/npm install --ignore-scripts
                    node /usr/bin/npm run build
                    
                    # Frontend
                    cd ../blog-frontend
                    node /usr/bin/npm install
                    
                    # Create .env file with OpenSSL fix
                    echo "GENERATE_SOURCEMAP=false" > .env
                    echo "NODE_OPTIONS=--openssl-legacy-provider" >> .env
                    
                    node /usr/bin/npm run build
                    
                    echo "✅ Build Complete!"
                '''
            }
        }
    }
}
