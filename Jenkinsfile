pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'myapp:latest'
	GHCR_REPO = 'ghcr.io/gilberto-s/ads-helloworld'
    }

    stages {
        stage('Checkout') {
            steps {
               git branch: 'main', url:'https://github.com/gilberto-s/ads-helloworld.git'
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
                    withCredentials([string(credentialsId: 'github', variable: 'GITHUB_TOKEN')]) {
                        sh '''
                        echo $GITHUB_TOKEN | docker login ghcr.io -u gilberto-s --password-stdin
                        docker tag myapp:latest $GHCR_REPO:$BUILD_NUMBER
                        docker push $GHCR_REPO:$BUILD_NUMBER
                        docker tag myapp:latest $GHCR_REPO:latest
                        docker push $GHCR_REPO:latest
                        '''
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

