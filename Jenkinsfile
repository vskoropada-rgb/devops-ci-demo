pipeline {
    agent any

    environment {
        IMAGE = "devops-demo"
    }

    stages {

        stage('Clone repo') {
            steps {
                sh '''
                rm -rf app
                git clone git@github.com:vskoropada-rgb/devops-ci-demo.git app
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    VERSION = sh(
                        script: "git describe --tags --abbrev=0 || echo latest",
                        returnStdout: true
                    ).trim()

                    sh """
                    cd app
                    docker build -t ${IMAGE}:${VERSION} .
                    docker tag ${IMAGE}:${VERSION} ${IMAGE}:latest
                    """
                }
            }
        }

        stage('Deploy to App Server') {
            steps {
                script {

                    sh """
                    docker save ${IMAGE}:latest | ssh sc@192.168.64.16 docker load

                    ssh sc@192.168.64.16 'docker stop devops-demo || true'
                    ssh sc@192.168.64.16 'docker rm devops-demo || true'

                    ssh sc@192.168.64.16 'docker run -d -p 5000:5000 --name devops-demo ${IMAGE}:latest'
                    """
                }
            }
        }

    }
}
