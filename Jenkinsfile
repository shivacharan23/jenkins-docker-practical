
pipeline {
    agent any

    stages {

        stage('Create Project') {
            steps {
                echo 'Creating HTML project...'

                writeFile file: 'index.html', text: '''
<html>
<head>
    <title>Jenkins Docker Project</title>
</head>
<body>
    <h1>Welcome to My Website</h1>
    <p>Successfully deployed using Jenkins and Docker!</p>
</body>
</html>
'''

                writeFile file: 'Dockerfile', text: '''
FROM nginx:latest
COPY index.html /usr/share/nginx/html/index.html
EXPOSE 80
'''
            }
        }

        stage('Check Docker') {
            steps {
                echo 'Checking Docker installation...'
                bat 'docker --version'

                echo 'Checking Docker Engine...'
                bat 'docker info'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo 'Building Docker image...'
                bat 'docker build -t mywebsite .'
            }
        }

        stage('Run Docker Container') {
            steps {
                echo 'Removing previous container if present...'
                bat 'docker rm -f mywebsite-container 2>NUL || exit /b 0'

                echo 'Starting new container...'
                bat 'docker run -d --name mywebsite-container -p 8081:80 mywebsite'
            }
        }

        stage('Verify Deployment') {
            steps {
                echo 'Checking running containers...'
                bat 'docker ps'

                echo 'Website deployed successfully!'
            }
        }
    }

    post {
        success {
            echo 'PIPELINE EXECUTED SUCCESSFULLY!'
        }

        failure {
            echo 'PIPELINE FAILED!'
        }

        always {
            echo 'Pipeline execution completed.'
        }
    }
}

#Docker
mkdir docker-compose-practical
cd docker-compose-practical

nano index.html

<!DOCTYPE html>
<html>
<head>
    <title>Docker Compose Practical</title>
</head>
<body>
    <h1>Welcome to Docker Compose</h1>
    <p>Nginx and PostgreSQL are running!</p>
</body>
</html>

nano docker-compose.yml


services:
  web:
    image: nginx:latest
    container_name: nginx-server
    ports:
      - "8082:80"
    volumes:
      - ./index.html:/usr/share/nginx/html/index.html:ro
    depends_on:
      - db

  db:
    image: postgres:16
    container_name: postgres-db
    environment:
      POSTGRES_USER: admin
      POSTGRES_PASSWORD: password123
      POSTGRES_DB: mydatabase
    ports:
      - "5432:5432"

docker compose up -d

docker ps
