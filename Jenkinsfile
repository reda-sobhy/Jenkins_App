pipeline {
    agent { label 'ec2' }

    tools {
        jdk 'jdk'
        maven 'maven'
    }

    stages {
        stage('Checkout Code') {
            steps {
                // نعمل clone للمستودع من GitHub
                git url: 'https://github.com/reda-sobhy/Jenkins_App.git', branch: 'main'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean verify'
            }
        }

        stage('OWASP Scan') {
            steps {
                dependencyCheck additionalArguments: '--scan ./ --format XML --format HTML', odcInstallation: 'dp'
                dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
            }
        }
    }
}
