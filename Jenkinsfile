pipeline {
    agent any

    stages {

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t devops-demo .'
            }
        }

        stage('Run Container') {
            steps {
                sh '''
                docker rm -f devops-demo || true
                docker run -d -p 5000:5000 --name devops-demo devops-demo
                '''
            }
        }

    }
}