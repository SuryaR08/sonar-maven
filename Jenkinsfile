pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build & Test') {
            steps {
                sh 'mvn clean verify'
            }
        }
        stage('SonarAnalysis') {
            environment {
                SONAR_TOKEN = credentials('sonarqube-token')
            }
            steps {
                bat '''
                mvn clean verify sonar:sonar \
                -Dsonar.projectKey=sonarmaven \
                -Dsonar.projectName='sonarmaven' \
                -Dsonar.host.url=http://localhost:9000 \
                -Dsonar.token=sonarqube-token
                '''
            }
        }
    }
    post {
        success {
            echo 'Build and SonarQube analysis succeeded!'
        }
        failure {
            echo 'Build failed!'
        }
    }
}
