pipeline {
    agent any

    tools {
        jdk 'jdk17' // Replace with the actual JDK installation name in Jenkins
        maven 'sonarmaven' // Replace with the actual Maven installation name in Jenkins
    }

    environment {
        SONARQUBE_SERVER = 'Kavyashree TR' // Name of the SonarQube server configured in Jenkins
        SONARQUBE_TOKEN = credentials('secret') // Replace 'secret' with the ID of your SonarQube token in Jenkins credentials
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout source code from SCM
                checkout scm
            }
        }

        stage('Build') {
            steps {
                // Run Maven clean and package
                bat 'mvn clean package'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                // Perform SonarQube analysis
                withSonarQubeEnv('Kavyashree TR') {
                    bat """mvn sonar:sonar \
                        -Dsonar.projectKey=my-java-project \
                        -Dsonar.login=$SONARQUBE_TOKEN"""
                }
            }
        }

        stage('Quality Gate') {
            steps {
                // Wait for SonarQube quality gate
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
