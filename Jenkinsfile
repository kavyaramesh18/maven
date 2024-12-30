pipeline {
    agent any
    
    tools {
        // Using the custom Maven installation name
        jdk 'jdk17'
        maven 'sonarmaven'
    }

    environment {
        // Replace with your SonarQube server and token
        SONAR_HOST_URL = 'http://localhost:9000'
        SONAR_TOKEN = 'sqp_51f8f9f908fdf5f9e4cec9621f701421af71f6a2'
    }

    stages {
        stage('Checkout Code') {
            steps {
                echo 'Checking out code from Git...'
                git credentialsId: 'github_token', url: 'https://github.com/kavyaramesh18/maven.git'
            }
        }
        
        stage('Install Dependencies') {
            steps {
                echo 'Installing dependencies using Maven...'
                bat 'mvn clean install'
            }
        }

        stage('Run Tests') {
            steps {
                echo 'Running unit tests...'
                bat 'mvn test'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                echo 'Performing SonarQube analysis using sonar-scanner...'
                bat '''
                    sonar-scanner.bat \
                    -D"sonar.projectKey=NewMaven" \
                    -D"sonar.sources=." \
                    -D"sonar.host.url=http://localhost:9000" \
                    -D"sonar.token=sqp_51f8f9f908fdf5f9e4cec9621f701421af71f6a2"
                '''
            }
        }
    }

    post {
        always {
            echo 'Cleaning up...'
            deleteDir()
        }
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check logs for more details.'
        }
    }
}
