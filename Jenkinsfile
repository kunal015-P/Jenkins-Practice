pipeline {
    agent any

    environment {
        AWS_DEFAULT_REGION = "us-west-1"
    }

    stages {

        stage('Verify AWS Identity') {
            steps {
                withCredentials([[
                    $class: 'AmazonWebServicesCredentialsBinding',
                    credentialsId: 'aws-jenkins-creds'
                ]]) {
                    sh 'aws sts get-caller-identity'
                }
            }
        }

        stage('Create S3 Bucket (us-west-1)') {
            steps {
                withCredentials([[
                    $class: 'AmazonWebServicesCredentialsBinding',
                    credentialsId: 'aws-jenkins-creds'
                ]]) {
                    sh '''
                      BUCKET_NAME=jenkins-usw1-$BUILD_NUMBER-$RANDOM
                      echo "Creating bucket: $BUCKET_NAME in us-west-1"

                      aws s3api create-bucket \
                        --bucket $BUCKET_NAME \
                        --region us-west-1 \
                        --create-bucket-configuration LocationConstraint=us-west-1
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
            echo "✅ S3 bucket created in us-west-1"
        }
        failure {
            echo "❌ Failed to create S3 bucket"
        }
    }
}


