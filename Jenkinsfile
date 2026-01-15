pipeline {
    agent any
    
    stages {
        stage('Build') {
            steps {
                sh '''
                    # Build Backend
                    cd blog-backend
                    node /usr/bin/npm install --ignore-scripts
                    ./node_modules/.bin/tsc --skipLibCheck
                    
                    # Build Frontend
                    cd ../blog-frontend
                    node /usr/bin/npm install
                    NODE_OPTIONS="--openssl-legacy-provider" node /usr/bin/npm run build
                '''
                
                // SINGLE STASH COMMAND FOR BOTH
                stash name: 'all-artifacts', 
                      includes: 'blog-backend/dist/**/*,blog-backend/package.json,blog-frontend/build/**/*'
            }
        }
        
        stage('Deploy') {
            steps {
                // SINGLE UNSTASH FOR BOTH
                unstash 'all-artifacts'
                
                sh '''
                    # Deploy Backend
                    cd blog-backend
                    npm install --production
                    pm2 delete blog-backend 2>/dev/null || true
                    pm2 start dist/main.js --name blog-backend
                    
                    # Deploy Frontend
                    cd ../blog-frontend
                    cp -r build/* /var/www/html/ 2>/dev/null || echo "Copy attempted"
                    sudo nginx -s reload 2>/dev/null || true
                    
                    echo "✅ Deployed from stashed artifacts!"
                '''
            }
        }
    }
}
