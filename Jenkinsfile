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
                sh 'docker stop $(docker ps -qf ancestor=application)'
                sh 'docker run --rm -d -p 9889:80 application'
            }
        }
    }
}
