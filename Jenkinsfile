pipeline {
    agent any

    triggers {
        cron('H 10 * * *')
    }

    stages {
        stage('Clone from GitHub') {
            steps {
                echo 'Cloning repository...'
                git branch: 'main', url: 'https://github.com/AlsayedAldahawy/jenkins-task.git'
            }
        }

        stage('Build Docker Image') {
            when {
                expression { fileExists('Dockerfile') }
            }
            steps {
                echo 'Building Docker image...'
                bat 'docker build -t jenkins-python-app .'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Running Python app inside Docker...'
                script {
                    try {
                        if (fileExists('Dockerfile')) {
                            bat 'docker run --rm jenkins-python-app'
                        } else {
                            bat 'python app.py'
                        }
                        echo '✅ Deployment successful!'
                    } catch (err) {
                        echo "❌ Deployment failed: ${err}"
                        error("Deployment failed!")
                    }
                }
            }
        }
    }

    post {
        always {
            echo 'Cleaning up workspace...'
            deleteDir()
        }
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
