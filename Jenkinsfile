pipeline {
    // If your Jenkins agent has a specific label 'node', keep it. 
    // Otherwise, change this to 'agent any' if it hangs on execution.
    agent any  
    
    stages {
        stage('Checkout') {
            steps {
                echo 'Code Checkout...'
                // Fixed: Changed branch from 'master' to 'main' to match your GitHub push target!
                git url: 'https://github.com/isitkk/portfolio-website.git', branch: 'main'
            }
        }
        
        stage('Build') {
            steps { 
                echo 'Building the Docker Image...' 
                sh 'docker build -t portfolio:latest .'
                echo 'Code Built Successfully!'
            }
        }
        
        stage('Push to Dockerhub') {
            steps {
                echo 'Pushing the image to Docker Hub...' 
                // Ensure the credential ID 'docker-cred' exists exactly as typed in your Jenkins settings
                withCredentials([usernamePassword(credentialsId: 'docker-cred', 
                                  usernameVariable: 'dockeruser', passwordVariable: 'dockerpass')]) {
                    sh 'docker image tag portfolio:latest $dockeruser/portfolio:latest'
                    sh 'docker login -u $dockeruser -p $dockerpass'
                    sh 'docker push $dockeruser/portfolio:latest'
                }
            }
        }
        
        stage('Deploy') {
            steps {
                echo 'Deploying Container...'
                // 1. Safely clear out the specific old container if it exists, ignoring errors if it doesn't
                sh 'docker stop portfolio-prod || true'
                sh 'docker rm portfolio-prod || true'
                
                // 2. Fixed name syntax error (removed dot) and shifted host port to 8081 to avoid root permission issues
                sh 'docker run --name portfolio-prod -d -p 8081:5173 portfolio:latest'
                
                echo 'Portfolio deployed successfully! Accessible at http://localhost:8081'
            }
        }
    }
}
