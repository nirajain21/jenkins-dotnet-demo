pipeline {
    agent any

    stages {
        stage('Restore') {
            steps {
                echo 'Restoring dependencies...'
                sh '/usr/local/share/dotnet/dotnet restore "TASK 6.2HD.sln"'
            }
        }

        stage('Build') {
            steps {
                echo 'Building the project...'
                sh '/usr/local/share/dotnet/dotnet build "TASK 6.2HD.sln" -c Release --no-restore'
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                sh '/usr/local/share/dotnet/dotnet test "TASK 6.2HD.sln" --no-build --verbosity normal'
            }
        }

        stage('Publish') {
            steps {
                echo 'Publishing the project...'
                sh '/usr/local/share/dotnet/dotnet publish "TASK 6.2HD.sln" -c Release -o ./publish'
            }
        }

        stage('Docker Build and Deploy') {
            steps {
                echo 'Building Docker image...'
                sh 'docker build -t dotnet-app-image .' // Build the Docker image from the Dockerfile

                echo 'Running Docker container...'
                sh 'docker stop dotnet-app-container || true && docker rm dotnet-app-container || true' // Stop and remove any existing container with the same name
                sh 'docker run -d -p 8080:80 --name dotnet-app-container dotnet-app-image' // Run the container, mapping port 8080 on the host to port 80 on the container
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
