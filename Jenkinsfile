pipeline {
    agent any

    stages {
        stage('Build') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    ls -la
                    node --version
                    npm --version
                    
                    # Clear npm cache and ensure correct permissions
                    npm cache clean --force
                    
                    # Install dependencies with more verbose output
                    npm install --verbose
                    
                    # Ensure react-scripts is installed properly
                    npm list react-scripts || npm install react-scripts --save-dev
                    
                    # Run the build
                    npm run build
                    
                    # List build artifacts
                    ls -la build/
                '''
            }
        }
    }
}
