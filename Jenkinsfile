
pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/kavyasri59/jenkins-docker-cicd.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build --pull -t jenkins-docker-cicd:latest .'
            }
        }

        stage('Test Application') {
            steps {
                sh '''
                    set -e
                    test -s index.html
                    grep -qi '<html' index.html
                    grep -qi '</html>' index.html
                    grep -qi '</body>' index.html
                    docker image inspect jenkins-docker-cicd:latest >/dev/null
                    echo "All application checks passed!"
                '''
            }
        }

        stage('Security Scan') {
            steps {
                sh '''
                    set -e
                    trivy image \
                      --severity HIGH,CRITICAL \
                      --exit-code 1 \
                      jenkins-docker-cicd:latest
                '''
            }
        }

        stage('Deploy Container') {
            steps {
                sh '''
                    docker rm -f jenkins-demo 2>/dev/null || true
                    docker run -d \
                        --name jenkins-demo \
                        --restart unless-stopped \
                        -p 8081:80 \
                        jenkins-docker-cicd:latest
                '''
            }
        }
    }
}
