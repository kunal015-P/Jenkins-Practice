pipeline {
    agent any

    environment {
        AWS_DEFAULT_REGION = 'us-west-1'
        VPC_NAME = 'My-VPC'
        BUCKET_NAME = '153185gdf'
    }

    stages {

        stage('Create VPC') {
            steps {
                sh '''
                echo "Creating VPC..."

                VPC_ID=$(aws ec2 create-vpc \
                  --cidr-block 10.0.0.0/16 \
                  --query 'Vpc.VpcId' \
                  --output text)

                echo "VPC created: $VPC_ID"

                aws ec2 create-tags \
                  --resources $VPC_ID \
                  --tags Key=Name,Value=$VPC_NAME
                '''
            }
        }

        stage('Create S3 Bucket') {
            steps {
                sh '''
                echo "Creating S3 bucket..."

                aws s3api create-bucket \
                  --bucket $BUCKET_NAME \
                  --region $AWS_DEFAULT_REGION \
                  --create-bucket-configuration LocationConstraint=$AWS_DEFAULT_REGION

                echo "S3 bucket created: $BUCKET_NAME"
                '''
            }
        }
    }
}
