
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
                sh 'docker build -t jenkins-docker-cicd:latest .'
            }
        }

        stage('Test Application') {
            steps {
                sh '''
                    set -e

                    echo "Checking index.html exists..."
                    test -s index.html

                    echo "Checking HTML structure..."
                    grep -qi '<html' index.html
                    grep -qi '</html>' index.html
                    grep -qi '</body>' index.html

                    echo "Checking Docker image exists..."
                    docker image inspect jenkins-docker-cicd:latest > /dev/null

                    echo "All application checks passed!"
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
