pipeline {
    agent {label "Slave"}
    environment {
        APP_NAME = "register-app-pipeline"
    }


    stages {
        stage("Checkout") {
            steps {
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/ridodomayana/Register-Ops-CD.git'
            }
        }
        stage("Validating YAML Files") {
            steps {
                echo 'Validating Kubernetes YAML files...'
                sh 'kubectl apply --dry-run=client -f regapp-deploy.yml'
                sh 'kubectl apply --dry-run=client -f regapp-service.yml'
            }
        }
        stage("Apply YAML Files") {
            steps {
                echo 'Applying Kubernetes YAML files...'
                sh '''
                kubectl apply -f regapp-deploy.yml
                kubectl apply -f regapp-service.yml
                '''
            }
        }
    }
    post {
        success {
            echo 'Deployment completed successfully!'
        }
        failure {
            echo 'Deployment failed. Please check logs.'
        }
    }
}
