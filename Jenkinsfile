pipeline {
    agent {
       docker { image 'mcr.microsoft.com/dotnet/sdk:8.0' }
    }

    environment {
        DOTNET_VERSION = '8.0' // Set the version of the .NET SDK you're using
        BUILD_DIR = 'build' // Define the build directory
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out the source code'
                checkout scm
            }
        }

        stage('Restore Dependencies') {
            steps {
                echo 'Restoring dependencies'
                sh "dotnet restore"
            }
        }

        stage('Build') {
            steps {
                echo 'Building the project'
                sh "dotnet build --configuration Release --no-restore"
            }
        }

        stage('Test') {
            steps {
                echo 'Running unit tests'
                sh "dotnet test --configuration Release --no-build"
            }
        }

        stage('Publish') {
            steps {
                echo 'Publishing the application'
                sh "dotnet publish --configuration Release --output ${BUILD_DIR}"
            }
        }

        stage('Deploy') {
            when {
                branch 'main'
            }
            steps {
                echo 'Deploying the application'
                // Example: Copy files to a remote server
                sh "scp -r ${BUILD_DIR}/* user@your-server:/var/www/your-app"
                // You can replace this with your actual deployment process (e.g., Kubernetes, Docker, etc.)
            }
        }
    }

    post {
        success {
            echo 'Pipeline succeeded!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
