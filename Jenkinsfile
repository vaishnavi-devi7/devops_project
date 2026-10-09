pipeline {
    agent any
    environment {
        PATH = "/opt/homebrew/bin:/usr/local/bin:${env.PATH}"
        IMAGE_NAME = "ghcr.io/vaishnavi-devi7/devops_project:latest"
        GHCR_CREDS = credentials('github-token') 
    }
    stages {
        stage('Job 1: Build & Push Docker Image') {
            steps {
                script {
                    sh 'docker build --platform linux/amd64 -t $IMAGE_NAME .'
                    sh 'echo $GHCR_CREDS_PSW | docker login ghcr.io -u $GHCR_CREDS_USR --password-stdin'
                    sh 'docker push $IMAGE_NAME'
                }
            }
        }
        stage('Job 2: Terraform Provisioning') {
            steps {
                dir('terraform') {
                    sh 'terraform init'
                    sh 'terraform validate'
                    sh 'terraform plan'
                    sh 'terraform apply -auto-approve'
                }
            }
        }
    }
}