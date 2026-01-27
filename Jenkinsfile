pipeline {
    agent any
    stages {
        stage('SonarQube Analysis') {
            steps {
                script {
                    
                    def scannerHome = tool name: 'SonarQube Scanner'
                    , type: 'hudson.plugins.sonar.SonarRunnerInstallation'
                    withSonarQubeEnv('MySonarQube') {
                        sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=my-project"
                    }
                }
            }
        }
    }
}
