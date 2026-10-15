pipeline {
    agent any

    stages {
        stage('Checkout GitHub') {
            steps {
                checkout scm
            }
        }

        stage('Build Website Image') {
            steps {
                sh 'docker build -t mevi-html-site:1.0 .'
            }
        }

        stage('Deploy Website') {
            steps {
                sh '''
                    docker rm -f mevi-html-site 2>/dev/null || true
                    docker run -d --name mevi-html-site \
                        --restart unless-stopped \
                        -p 8082:80 mevi-html-site:1.0
                '''
            }
        }

        stage('Verify Website') {
            steps {
                sh 'curl -f http://host.docker.internal:8082/'
            }
        }
    }
}