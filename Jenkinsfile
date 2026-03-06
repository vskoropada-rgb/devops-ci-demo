pipeline {
    agent any

    stages {

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t devops-demo .'
            }
        }

        stage('Deploy to App Server') {
            steps {
                sh '''
                docker save devops-demo | ssh sc@192.168.64.16 docker load
                ssh sc@192.168.64.16 docker rm -f devops-demo || true
                ssh sc@192.168.64.16 docker run -d -p 5000:5000 --name devops-demo devops-demo
                '''
            }
        }

    }
}
