pipeline {
    agent any
    environment {
        SONAR_TOKEN = credentials('sonarqube-token') // Add SonarQube token as a Jenkins credential
    }
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
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') { // Ensure you have configured SonarQube in Jenkins
                    sh '''
                    mvn sonar:sonar \
                      -Dsonar.projectKey=sonarmaven \
                      -Dsonar.projectName="sonarmaven" \
                      -Dsonar.host.url=http://localhost:9000 \
                      -Dsonar.token=${SONAR_TOKEN}
                    '''
                }
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
