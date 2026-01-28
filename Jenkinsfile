pipeline {
    agent { label 'ec2' }

    tools {
        jdk 'jdk'
        maven 'maven'
    }

    stages {
        stage('Checkout Code') {
            steps {
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
                sh '''
                    /opt/dependency-check/dependency-check/bin/dependency-check.sh \
                    --project "JavaApp" \
                    --scan ./ \
                    --format XML \
                    --format HTML \
                    --out ./dependency-check-report
                '''
                dependencyCheckPublisher pattern: 'dependency-check-report/dependency-check-report.xml'
            }
        }
    }
}
