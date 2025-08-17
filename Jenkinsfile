pipeline {
    agent {
        docker {
            image 'python:3.9'
            args '-u root'
        }
    }

    environment {
        IMAGE_NAME = "jenkins-flask-demo"
        DOCKER_REGISTRY = "mickey1920"
    }

    stages {
        stage('Clone') {
            steps {
                git branch: 'main', url: 'https://github.com/pattelakrishna/jenkins-flask-cicd.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh '''
                    pip install --upgrade pip --quiet
                    pip install -r flask-app/requirements.txt --quiet
                '''
            }
        }

        // ... rest unchanged
    }

    post {
        success {
            echo '✅ Deployment successful!'
        }
        failure {
            echo '❌ Build failed.'
        }
    }
}
