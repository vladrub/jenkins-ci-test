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

                    final Integer responseCode = sh(script: "curl -s -o /dev/null -w %{http_code} $url", returnStdout: true).trim()
                    
                    if (responseCode == 200) {
                        echo "SmokeTest прошел успешно!"
                    } else {
                        echo responseCode
                        sh 'exit 1'
                    }
                }
            }
        }
        stage("FingerprintTest") {
            steps {
                script {
                    final String url = "130.193.43.88:9889"

                    final String curlFingerprint = sh(script: "wget -q -O- $url | md5sum | cut -c 1-32", returnStdout: true).trim()
                    final String fileFingerprint = sh(script: "md5sum app/index.html | cut -c 1-32", returnStdout: true).trim()
                    
                    if (curlFingerprint == fileFingerprint) {
                        echo "FingerprintTest прошел успешно!"
                    } else {
                        echo "curlFingerprint: $curlFingerprint и fileFingerprint: $fileFingerprint не равны"
                        sh 'exit 1'
                    }
                }
            }
        }
    }
    
     post {
        success {            
            telegramSend(message: '${currentBuild.projectName} ${currentBuild.displayName} ${currentBuild.result}')
        }
        aborted {             
            telegramSend(message: "aborted")
        }
        failure {
            telegramSend(message: "failure")
        }
    }
}
