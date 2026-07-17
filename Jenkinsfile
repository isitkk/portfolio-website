pipeline {
    agent any

    stages {
        stage('Clone Repo') {
            steps {
                git branch: 'main', 
                url: 'https://github.com/isitkk/portfolio-website.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("portfolio-app")
                }
            }
        }

        stage('Run Container') {
            steps {
                script {
                    sh 'docker stop portfolio || true'
                    sh 'docker rm portfolio || true'
                    sh 'docker run -d -p 8081:5173 --name portfolio portfolio-app'
                }
            }
        }
    }
}
