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
                sed -e 's,{{password}},${MYSQL_ROOT_PASSWORD}',g;' db-password.yaml | kubectl apply -f -
                kubectl apply -f task2-app-manifest.yaml
                kubectl apply -f task2-db-manifest.yaml
                kubectl apply -f nginx-pod-manifest.yaml
                kubectl apply -f nginx-config-manifest.yaml
                sleep 60
                kubectl get services
                '''
            }

        }

    }

}