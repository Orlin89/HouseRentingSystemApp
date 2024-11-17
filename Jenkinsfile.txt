pipeline {
    agent any

    environment {
        DOTNET_VERSION = '6.0' // Specify the .NET version
    }

    stages {
        stage('Restore Dependencies') {
            steps {
                echo 'Restoring NuGet packages...'
                bat 'dotnet restore'
            }
        }
        stage('Build Application') {
            steps {
                echo 'Building the project...'
                bat 'dotnet build --no-restore'
            }
        }
        stage('Run Tests') {
            steps {
                echo 'Running tests...'
                bat 'dotnet test --no-build --verbosity normal'
            }
        }
    }

    post {
        always {
            echo 'Cleaning up workspace...'
            cleanWs()
        }
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Please check the logs for details.'
        }
    }
}
