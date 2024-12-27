pipeline {
    agent any
    environment {
        SONAR_TOKEN = credentials('sonarqube-token') 
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build & Test') {
            steps {
                script {
                    def mvnHome = tool 'sonarmaven' 
                    bat """
                    set PATH=%mvnHome%\\bin;%PATH%
                    mvn clean verify
                    """
                }
            }
        }
        stage('SonarQube Analysis') {
            steps {
                script {
                    def mvnHome = tool 'sonarmaven'
                    withSonarQubeEnv('SonarQube') { 
                        bat """
                        set PATH=%mvnHome%\\bin;%PATH%
                        mvn sonar:sonar ^
                          -Dsonar.projectKey=sonarmaven ^
                          -Dsonar.projectName="sonarmaven" ^
                          -Dsonar.host.url=http://localhost:9000 ^
                          -Dsonar.token=%SONAR_TOKEN%
                        """
                    }
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
