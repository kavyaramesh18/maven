pipeline {
    agent any

    tools {
        jdk 'jdk17'           // Replace with your JDK installation name in Jenkins
        maven 'sonarmaven'    // Replace with your Maven installation name in Jenkins
    }

    environment {
        SONARQUBE_SERVER = 'SonarQubeServer'       // Replace with the SonarQube server name configured in Jenkins
        SONARQUBE_TOKEN = credentials('newmaven')  // Replace 'newmaven' with the ID of your SonarQube token stored in Jenkins credentials
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout source code from SCM (Source Control Management)
                checkout scm
            }
        }

        stage('Build') {
            steps {
                // Run Maven clean and package
                sh 'mvn clean package'  // Use `sh` for Linux/Mac or `bat` for Windows
            }
        }

        stage('SonarQube Analysis') {
            steps {
                // Perform SonarQube analysis
                withSonarQubeEnv(SONARQUBE_SERVER) {
                    script {
                        def scannerHome = tool 'SonarScanner' // Replace with the SonarScanner tool name configured in Jenkins
                        sh """
                            ${scannerHome}/bin/sonar-scanner \
                            -Dsonar.projectKey=NewMaven \
                            -Dsonar.sources=. \
                            -Dsonar.host.url=http://localhost:9000 \
                            -Dsonar.login=${SONARQUBE_TOKEN}
                        """
                    }
                }
            }
        }

        stage('Quality Gate') {
            steps {
                // Wait for SonarQube Quality Gate status
                timeout(time: 1, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }

    post {
        always {
            // Archive the built artifacts
            archiveArtifacts artifacts: 'target/*.jar', allowEmptyArchive: true

            // Clean up the workspace
            cleanWs()
        }
        failure {
            echo 'Build failed!'
        }
        success {
            echo 'Build and SonarQube analysis successful!'
        }
    }
}
