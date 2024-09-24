pipeline {
    agent any

    stages {
        stage('Restore') {
            steps {
                echo 'Restoring dependencies...'
                sh 'dotnet restore "TASK 6.2HD.sln"'
            }
        }

        stage('Build') {
            steps {
                echo 'Building the project...'
                sh 'dotnet build "TASK 6.2HD.sln" -c Release --no-restore'
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                sh 'dotnet test "TASK 6.2HD.sln" --no-build --verbosity normal'
            }
        }

        stage('Publish') {
            steps {
                echo 'Publishing the project...'
                sh 'dotnet publish "TASK 6.2HD.sln" -c Release -o ./publish'
            }
        }
    }

    post {
        always {
            echo 'Cleaning up...'
            cleanWs()
        }
    }
}
