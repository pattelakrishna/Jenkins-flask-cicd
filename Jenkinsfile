pipeline {
    agent {
        docker { image 'python:3.9' }
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
                    pip install --upgrade pip --quite
                    pip install -r flask-app/requirements.txt --quite
                '''
            }
        }

        stage('Unit Test') {
            steps {
                sh 'pytest flask-app/test_app.py'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME ./flask-app'
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([string(credentialsId: 'dockerhub-token', variable: 'DOCKER_TOKEN')]) {
                    sh '''
                        echo $DOCKER_TOKEN | docker login -u $DOCKER_REGISTRY --password-stdin
                        docker tag $IMAGE_NAME $DOCKER_REGISTRY/$IMAGE_NAME:latest
                        docker push $DOCKER_REGISTRY/$IMAGE_NAME:latest
                    '''
                }
            }
        }

        stage('Deploy via Docker Compose') {
            steps {
                sh 'docker-compose down || true'
                sh 'docker-compose up -d --build'
            }
        }
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
