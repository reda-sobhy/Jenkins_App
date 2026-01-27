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

    }
