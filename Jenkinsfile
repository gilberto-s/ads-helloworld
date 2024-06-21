pipeline {
    agent {
        docker { image 'python:3.8' }
    }

    stages {
        stage('Build') {
            steps {
                echo 'Building...'
            }
        }

        stage('Test') {
            steps {
                echo 'Testing...'
                sh 'python3 hello.py'
            }
        }
    }
}


