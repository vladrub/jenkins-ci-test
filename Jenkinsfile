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
                sh 'docker ps -f ancestor=application -q | xargs --no-run-if-empty docker container stop'
                sh 'docker run --rm -d -p 9889:80 application'
            }
        }
    }
}
