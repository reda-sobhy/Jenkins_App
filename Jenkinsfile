pipeline {
    agent { label 'ec2' }

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
        dependencyCheck additionalArguments: '--scan ./ --format XML --format HTML', odcInstallation: 'DP'
        dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
    }
}
    }
}
