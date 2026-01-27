pipeline {
    agent any

    tools {
        jdk 'jdk'
        maven 'maven'
    }

    stages {
        stage('Build') {
            steps {
                sh 'mvn clean verify'
            }
        }
      

stage('OWASP Scan') {
    steps {
        dependencyCheck additionalArguments: '--scan ./src/main/java --format XML --format HTML', odcInstallation: 'DP'
        dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
    }
}
    }
}
