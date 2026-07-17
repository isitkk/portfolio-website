pipeline {
    agent { label 'node' }
    
    stages {
        stage('Chekout') {
            steps {
                echo 'Code Chckout'
                git url:'https://github.com/pritz45/portfolio.git', branch:'master'
            }
        }
        
        stage('Build') {
            steps { 
                echo 'Building the Code' 
                sh 'docker build -t portfolio:latest .'
                echo 'Code build Successfully'
            }
        }
        
        stage('Push to Dockerhub') {
            steps {
                echo 'Pushing the image to dockerhub' 
                withCredentials([usernamePassword(credentialsId:'docker-cred', 
                usernameVariable:'dockeruser', passwordVariable:'dockerpass')]) {
                sh 'docker image tag portfolio:latest $dockeruser/portfolio:latest'
                sh 'docker login -u $dockeruser -p $dockerpass'
                sh 'docker push $dockeruser/portfolio:latest'
                }
            }
        }
        
        stage('Deploy') {
            steps {
                echo 'Deploying Code'
                sh 'docker container prune -f'
                sh 'docker run --name port-cont. -d -p 80:5173 portfolio:latest'
            }
        }
    }
}
