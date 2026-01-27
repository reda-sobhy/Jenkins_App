pipeline {
    agent any
    tools {
        maven 'maven'
        
    }

    stages {
        stage('Build') {
            steps {
                sh 'mvn clean verify'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sonarqube') {
                    sh 'mvn sonar:sonar -Dsonar.projectKey=my-project'
                }
            }
        }

        stage('Quality Gate') {
            steps {
                waitForQualityGate abortPipeline: true
            }
        }
    }

    post {
        success {
            echo "Build and SonarQube Analysis succeeded ✅"
        }
        failure {
            echo "Pipeline failed ❌"
        }
    }
}
