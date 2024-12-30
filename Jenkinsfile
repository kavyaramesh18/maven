pipeline {
    agent any

    tools {
        jdk 'jdk17' // Replace with the actual JDK installation name in Jenkins
        maven 'sonarmaven' // Replace with the actual Maven installation name in Jenkins
    }

    environment {
        SONARQUBE_SERVER = 'Kavyashree TR' // Name of the SonarQube server configured in Jenkins
        SONARQUBE_TOKEN = credentials('newmaven') // Replace 'secret' with the ID of your SonarQube token in Jenkins credentials
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
                    bat """sonar-scanner.bat -D"sonar.projectKey=NewMaven" -D"sonar.sources=." -D"sonar.host.url=http://localhost:9000" -D"sonar.token=sqp_239bdf1e68af46dd88dfce2653520bb4223e7816""""
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
