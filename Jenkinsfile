pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out source code...'
                git 'https://github.com/nirajain21/jenkins-dotnet-demo.git'
            }
        }

        stage('Restore') {
            steps {
                echo 'Restoring dependencies...'
                sh '/bin/sh -c "/usr/local/share/dotnet/dotnet restore 6.2HD.csproj"'
            }
        }

        stage('Build') {
            steps {
                echo 'Building the project...'
                sh '/bin/sh -c "/usr/local/share/dotnet/dotnet build 6.2HD.csproj -c Release --no-restore"'
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                sh '/bin/sh -c "/usr/local/share/dotnet/dotnet test 6.2HD.csproj --no-build --verbosity normal"'
            }
        }

        stage('Publish') {
            steps {
                echo 'Publishing the project...'
                sh '/bin/sh -c "/usr/local/share/dotnet/dotnet publish 6.2HD.csproj -c Release -o ./publish"'
            }
        }

        stage('Docker Build and Deploy') {
            steps {
                echo 'Building Docker image...'
                sh 'docker build -t dotnet-app-image .'
                echo 'Running Docker container...'
                sh 'docker run -d -p 8080:80 --name dotnet-app-container dotnet-app-image'
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
