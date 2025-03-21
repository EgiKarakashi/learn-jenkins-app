pipeline {
    agent any

    stages {
        stage('Build') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                    args '--network host --dns 8.8.8.8 --dns 8.8.4.4'
                }
            }
            steps {
                sh '''
                    # Check connectivity
                    echo "Testing network connectivity..."
                    ping -c 2 registry.npmjs.org || echo "Ping failed but continuing"
                    
                    # Set npm timeouts and retries
                    npm config set fetch-retry-mintimeout 20000
                    npm config set fetch-retry-maxtimeout 120000
                    npm config set fetch-retries 5
                    
                    # Try using a different registry mirror if available
                    # npm config set registry https://registry.npmmirror.com/
                    
                    # Install dependencies with fallback strategies
                    echo "Installing dependencies..."
                    npm install --verbose --no-fund --no-audit || npm install --verbose --no-fund --no-audit --registry=https://registry.npmmirror.com/
                    
                    # Build the app
                    echo "Building the application..."
                    npm run build
                    
                    # List build artifacts
                    ls -la build/
                '''
            }
        }
    }
}