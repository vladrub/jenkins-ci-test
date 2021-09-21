pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                sh 'docker run --name application -d'
            }
        }
    }
}
