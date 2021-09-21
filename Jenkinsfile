pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                sh 'docker build -t application .'
            }
        }
        stage('Deploy') {
            steps {
                sh 'docker run --rm -d -p 9889:80 application'
            }
        }
    }
}
