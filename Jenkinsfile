pipeline {
    agent any

    tools {
        maven 'maven'  
         sonarScanner 'sonarqube'
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
                    sh """
                        sonar-scanner \
                        -Dsonar.projectKey=my-demo-app \
                        -Dsonar.sources=src/main/java/com/example/demo \
                        -Dsonar.java.binaries=target/classes
                    """
                }
            }
        }

        stage('Quality Gate') {
            steps {
                waitForQualityGate abortPipeline: true
            }
        }
    }
}
