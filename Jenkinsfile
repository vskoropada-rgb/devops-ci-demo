pipeline {
    agent any

    stages {

        stage('Clone repo') {
            steps {
                sh '''
                rm -rf app
                git clone git@github.com:vskoropada-rgb/devops-ci-demo.git app
                '''
            }
        }

        stage('Get Version') {
            steps {
                script {
                    VERSION = sh(
                        script: "cd app && git describe --tags --abbrev=0",
                        returnStdout: true
                    ).trim()

                    env.VERSION = VERSION
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                cd app
                docker build -t devops-demo:${VERSION} .
                docker tag devops-demo:${VERSION} devops-demo:latest
                '''
            }
        }

        stage('Deploy to App Server') {
            steps {
                sh '''
                docker save devops-demo:${VERSION} | ssh sc@192.168.64.16 docker load

                ssh sc@192.168.64.16 "docker stop devops-demo || true"
                ssh sc@192.168.64.16 "docker rm devops-demo || true"

                ssh sc@192.168.64.16 "docker run -d -p 5000:5000 --name devops-demo devops-demo:${VERSION}"
                '''
            }
        }

    }
}
