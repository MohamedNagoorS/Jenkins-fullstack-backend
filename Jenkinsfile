pipeline {
    agent any

    tools {
        nodejs 'sonarnode'  
    }
    
    environment {
        NODE_VERSION = '23'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Install') {
            steps {
                bat '''npm install
                npm run lint'''  
            }
        }

        // Removed Build stage as it is not needed for backend
        stage('SonarAnalysis') {
            environment {
                SONAR_TOKEN = credentials('sonarqube-token')  
            }
            steps {
                bat '''
                sonar-scanner ^
                -Dsonar.projectKey=backend ^
                -Dsonar.sources=. ^
                -Dsonar.host.url=http://localhost:9000 ^
                -Dsonar.token=sqp_48eaea52f16c865f2f4183dcd5f0a97f21b3d43b ^
                '''
            }
        }
    }

    post {
        success {
            echo "DONE SUCCESSFULLY"
        }
        failure {
            echo "SOMETHING IS WRONG"
        }
    }
}
