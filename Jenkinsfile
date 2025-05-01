pipeline {  
    agent any  
      
    stages {  
        stage('Build') {  
            steps {  
                sh 'docker build -t myapp:${BUILD_NUMBER} .'  
            }  
        }  
          
        stage('Test') {  
            steps {  
                sh 'docker run myapp:${BUILD_NUMBER} npm test'  
                // Or for more complex scenarios:  
                sh 'docker-compose -f docker-compose.test.yml up --abort-on-container-exit'  
            }  
        }  
          
        stage('Deploy') {  
            when {  
                branch 'main'  
            }  
            steps {  
                sh 'docker tag myapp:${BUILD_NUMBER} myapp:latest'  
                sh 'docker push myapp:latest'  
                sh 'kubectl apply -f k8s-deployment.yaml' // If using Kubernetes  
                // Or for direct Docker deployment:  
                sh 'docker-compose -f docker-compose.prod.yml up -d'  
            }  
        }  
    }  
}
