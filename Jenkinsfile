pipeline {
    agent any
    tools {
        maven 'maven'
    }
    environment {
        build_number = "${env.BUILD_ID}"
        AWS_ACCOUNT_ID="130465145438"
        AWS_DEFAULT_REGION="us-east-1"
        IMAGE_REPO_NAME="my-helmchart-maven-jenkins"
        IMAGE_TAG="latest"
        REPOSITORY_URI = "${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/${IMAGE_REPO_NAME}"
    }
    stages {
        stage ('git repo') {
            steps {
                git branch: 'main', url: 'https://github.com/sreekanthpv12/JenkinswithArgoCD.git'
            }
        }
        
        stage ('helm package ') {
            steps {
                sh "helm package ."
            }
        }
        stage ('login to aws ') {
            steps {
             withCredentials([aws(credentialsId: 'ecr-credential', accessKeyVariable: 'AWS_ACCESS_KEY_ID', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY')]) {
                        
                        
            sh "aws ecr get-login-password | helm registry login  --username AWS -p \$(aws ecr get-login-password --region us-east-1)  130465145438.dkr.ecr.us-east-1.amazonaws.com"
            }
         }
        }
        stage ('push helmchart to aws ecr ') {
            steps {
                withCredentials([aws(credentialsId: 'ecr-credential', accessKeyVariable: 'AWS_ACCESS_KEY_ID', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY')]) {
                sh "helm push helm-guestbook-0.1.0.tgz oci://130465145438.dkr.ecr.us-east-1.amazonaws.com" 
            }
        }
        }
        
        
        }
        
    }
