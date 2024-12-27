pipeline {
    agent any

    tools {
        jdk 'jdk17'
        maven 'sonarmaven'
    }

    environment {
        SONARQUBE_SERVER = 'Kavyashree TR' // Name of the SonarQube server in Jenkins
        SONARQUBE_TOKEN = credentials('secret') // SonarQube token added in Jenkins credentials
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                bat 'mvn clean package'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('Kavyashree TR') {
                    bat """mvn sonar:sonar -Dsonar.projectKey=my-java-project -Dsonar.login=$SONARQUBE_TOKEN"""
                }
            }
        }

        stage('Quality Gate') {
            steps {
                timeout(time: 1, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: '/target/*.jar', allowEmptyArchive: true
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
