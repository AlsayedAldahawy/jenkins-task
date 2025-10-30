pipeline {
    agent any

    environment {
        APP_NAME = "python-simple-app"
        DOCKER_IMAGE = "python-simple-app-image"
        CONTAINER_NAME = "python-simple-app-container"
    }

    triggers {
        cron('H 10 * * *')
        githubPush()
    }

    stages {
        stage('Clone from GitHub') {
            steps {
                git branch: 'main', url: 'https://github.com/AlsayedAldahawy/jenkins-task.git'
            }
        }

        stage('Build') {
            steps {
                echo "Installing Python dependencies..."
                sh 'pip install -r requirements.txt'
            }
        }

        stage('Optional: Build Docker Image') {
            when {
                expression { fileExists('Dockerfile') }
            }
            steps {
                echo "Building Docker image..."
                sh "docker build -t ${DOCKER_IMAGE} ."
            }
        }

        stage('Deploy') {
            steps {
                echo "Deploying application..."
                sh "nohup python app.py &"
            }
        }
    }

    post {
        always {
            echo 'Cleaning up workspace...'
            deleteDir()
        }
        success {
            echo 'Build & Deploy completed successfully!'
        }
        failure {
            echo 'Build failed!'
        }
    }
}
