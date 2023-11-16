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
                docker build -t pixcs13/task2-nginx nginx
                '''
            }

        }
        stage('Push') {
            steps {
                sh '''
                docker push pixcs13/task2-app
                docker push pixcs13/task2-db
                docker push pixcs13/task2-nginx
                '''
            }

        }
        stage('Deploy') {
            steps {
                sh '''
                ssh jenkins@maria-deploy <<EOF
                export MYSQL_ROOT_PASSWORD=${YOUR_NAME}
                docker network rm task2-net && echo "removed network" || echo "network already removed"
                docker network create task2-net
                docker stop nginx && echo "Stopped nginx" || echo "nginx not running"
                docker rm nginx && echo "removed nginx" || echo "nginx does not exist"
                docker stop flask-app && echo "Stopped flask-app" || echo "flask-app not running"
                docker rm flask-app && echo "removed flask-app" || echo "flask-app does not exist"
                docker stop mysql && echo "Stopped flask-app" || echo "mysql not running"
                docker rm mysql && echo "removed mysql" || echo "mysql does not exist"
                docker run -d --name mysql --network task2-net -e MYSQL_ROOT_PASSWORD=${YOUR_NAME} pixcs13/task2-db
                docker run -d --name flask-app --network task2-net -e MYSQL_ROOT_PASSWORD=${YOUR_NAME} pixcs13/task2-app
                docker run -d --name nginx --network task2-net -p 80:80 pixcs13/task2-nginx
                '''
            }

        }

    }

}