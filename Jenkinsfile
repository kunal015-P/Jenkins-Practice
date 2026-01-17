pipeline {
    agent any

    environment {
        AWS_DEFAULT_REGION = "us-west-1"
        BUCKET_NAME = "jenkins-practice-${env.BUILD_NUMBER}"
    }

    stages {

        stage('Create S3 Bucket') {
            steps {
                withCredentials([[
                    $class: 'AmazonWebServicesCredentialsBinding',
                    credentialsId: 'aws-jenkins-creds'
                ]]) {
                    sh '''
                      aws s3 mb s3://$BUCKET_NAME
                    '''
                }
            }
        }

        stage('Verify Bucket') {
            steps {
                sh 'aws s3 ls'
            }
        }
    }

    post {
        success {
            echo "✅ S3 bucket created successfully"
        }
        failure {
            echo "❌ Failed to create S3 bucket"
        }
    }
}

