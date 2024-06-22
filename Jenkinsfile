pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'myapp:latest'
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/gilberto-s/ads-helloworld.git'
            }
        }
        stage('Build') {
            steps {
                script {
                    docker.build(DOCKER_IMAGE)
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    docker.image(DOCKER_IMAGE).inside {
                        sh 'pytest'
                    }
                }
            }
        }
        stage('Lint') {
            steps {
                script {
                    docker.image(DOCKER_IMAGE).inside {
                        sh 'pylint hello.py'
                    }
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'dockerhub-credentials') {
                        docker.image(DOCKER_IMAGE).push('latest')
                    }
                }
            }
        }
    }
    post {
        always {
            cleanWs()
        }
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}

