   pipeline {
       agent any

       tools {
           maven 'Maven3'
           jdk 'Java17'
       }

       stages {
           stage('Checkout') {
               steps {
                   echo 'Checking out code...'
                   git branch: 'main', 
                      url: 'https://github.com/AbhinayThakur/demoCiCd.git'
                   sh 'git status'
                   sh 'ls -la'
               }
           }
           stage('Build') {
               steps {
                   echo 'Building application...'
                   sh './mvnw clean package -DskipTests'
               }
           }
           stage('Test') {
               steps {
                   echo 'Running tests...'
                   sh './mvnw test'
               }
           }
           stage('Build Docker Image') {
               steps {
                   echo 'Building Docker image...'
                   sh 'docker build -t demo-ci-cd:latest .'
               }
           }
           stage('Deploy to Local') {
               steps {
                   echo 'Deploying container...'
                   sh 'docker stop demo-ci-cd || true'
                   sh 'docker rm demo-ci-cd || true'
                   sh 'docker run -d -p 8081:8080 --name demo-ci-cd demo-ci-cd:latest'
               }
           }
       }
       post {
           always {
               echo 'Pipeline completed. Check the logs above for details.'
           }
       }
   }