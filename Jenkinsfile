pipeline {
    agent any
    stages {
        stage('SonarQube Analysis') {
            steps {
               
                    
                
                    withSonarQubeEnv('sonarqube') {
                        sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=my-project"
                    }
            
            }
        }
    }
}
