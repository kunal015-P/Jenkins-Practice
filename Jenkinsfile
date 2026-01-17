pipeline {
    agent any

    environment {
        APP_NAME = "JenkinsPracticeApp"
    }

    stages {

        stage('Checkout Code') {
            steps {
                echo 'Checking out source code...'
                checkout scm
            }
        }

	stage('Testing') {
	    steps {
		echo 'I am testing the Production'
		checkout scm
	    }
	}

        stage('Build') {
            steps {
                echo "Building ${APP_NAME}"
                sh '''
                  echo "Simulating build..."
                  sleep 2
                '''
            }
        }

        stage('Test') {
            steps {
                echo "Running tests"
                sh './test.sh'
            }
        }

        stage('Package') {
            steps {
                echo "Packaging application"
                sh '''
                  tar -czf app.tar.gz app.sh
                '''
            }
        }
    }

    post {
        success {
            echo "Pipeline completed successfully 🎉"
        }
        failure {
            echo "Pipeline failed ❌"
        }
        always {
            echo "Cleaning up workspace"
            cleanWs()
        }
    }
}
