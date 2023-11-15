pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh '''
                docker build -t pixcs13/task1jenk .
                '''
            }

        }
        stage('Push') {
            steps {
                sh '''
                docker push pixcs13/task1jenk
                '''
            }

        }
        stage('Deploy') {
            steps {
                sh '''
                docker stop task1 && echo "Stopped taks1£ || echo "task1 not running"
                docker rm task1 && echo "removed taks1£ || echo "task1 does not exist"
                docker run -d -p 80:5500 --name task1 pixcs13/task1jenk
                '''
            }

        }

    }

}