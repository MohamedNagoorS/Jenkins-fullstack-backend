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
                sonar-scanner.bat -D"sonar.projectKey=backend" -D"sonar.sources=." -D"sonar.host.url=http://localhost:9000" -D"sonar.token=${SONAR_TOKEN}"
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
