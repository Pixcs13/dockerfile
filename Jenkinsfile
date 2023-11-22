pipeline {
    agent any
    environment {
        YOUR_NAME = credentials("YOUR_NAME")
    }
    stages {
        stage('Build') {
            steps {
                sh '''
                docker build -t pixcs13/task2-app flask-app
                docker build -t pixcs13/task2-db db
                '''
            }

        }
        stage('Push') {
            steps {
                sh '''
                docker push pixcs13/task2-app
                docker push pixcs13/task2-db
                '''
            }

        }
        stage('Deploy') {
            steps {
                sh '''
                kubectl apply -f .
                sleep 60
                kubectl get services
                '''
            }

        }

    }

}