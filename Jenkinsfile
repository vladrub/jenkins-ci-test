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
                sh 'docker ps -f name=application -q | xargs --no-run-if-empty docker container stop'
                sh 'docker run --name application --rm -d -p 9889:80 application'
            }
        }
        stage("SmokeTest") {
            steps {
                script {
                    final String url = "130.193.43.88:9889"

                    final String responseCode = sh(script: " curl -s -o /dev/null -w %{http_code} $url", returnStdout: true).trim()

                    echo responseCode
                    
                    if (responseCode != 201) {
                        sh 'exit 0'
                    }
                }
            }
        }
    }
}
