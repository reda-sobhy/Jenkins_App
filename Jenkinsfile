pipeline {
    agent any

    tools {
        jdk 'jdk'
        maven 'maven'
    }
   environment {
        AWS_REGION = "us-east-1"
        ECR_REPO   = "123456789012.dkr.ecr.us-east-1.amazonaws.com/my-app"
        IMAGE_TAG  = "${BUILD_NUMBER}"
        CLUSTER    = "my-eks-cluster"
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

    //     stage('OWASP Scan') {
    //         steps {
    //             sh '''
    //                 /opt/dependency-check/dependency-check/bin/dependency-check.sh \
    //                 --project "JavaApp" \
    //                 --scan ./ \
    //                 --format XML \
    //                 --format HTML \
    //                 --out ./dependency-check-report
    //             '''
    //             dependencyCheckPublisher pattern: 'dependency-check-report/dependency-check-report.xml'
    //         }
    //     }
    // }

    steps {
                sh """
                docker build -t my-app:${IMAGE_TAG} .
                docker tag my-app:${IMAGE_TAG} ${ECR_REPO}:${IMAGE_TAG}
                """
            }
        }

        stage('Trivy Scan') {
            steps {
                sh """
                trivy image --exit-code 1 --severity HIGH,CRITICAL my-app:${IMAGE_TAG}
                """
            }
        }

        stage('Login to ECR') {
            steps {
                withCredentials([
                    string(credentialsId: 'aws-access-key-id', variable: 'AWS_ACCESS_KEY_ID'),
                    string(credentialsId: 'aws-secret-access-key', variable: 'AWS_SECRET_ACCESS_KEY')
                ]) {
                    sh """
                    aws ecr get-login-password --region ${AWS_REGION} \
                    | docker login --username AWS --password-stdin ${ECR_REPO}
                    """
                }
            }
        }

        stage('Push Image to ECR') {
            steps {
                sh """
                docker push ${ECR_REPO}:${IMAGE_TAG}
                """
            }
        }

        stage('Deploy to EKS') {
            steps {
                withCredentials([
                    string(credentialsId: 'aws-access-key-id', variable: 'AWS_ACCESS_KEY_ID'),
                    string(credentialsId: 'aws-secret-access-key', variable: 'AWS_SECRET_ACCESS_KEY')
                ]) {
                    sh """
                    aws eks update-kubeconfig --region ${AWS_REGION} --name ${CLUSTER}
                    sed -i 's|IMAGE_PLACEHOLDER|${ECR_REPO}:${IMAGE_TAG}|g' deployment.yaml
                    kubectl apply -f deployment.yaml
                    """
                }
            }

}
}
