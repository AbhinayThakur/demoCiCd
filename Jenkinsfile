pipeline {
    agent any

    tools {
        maven 'Maven3'
        jdk 'Java17'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/AbhinayThakur/demoCiCd'
            }
        }
        stage('Build') {
            steps {
                sh './mvnw clean package -DskipTests'
            }
        }
        stage('Test') {
            steps {
                sh './mvnw test'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t demo-ci-cd:latest .'
            }
        }
        stage('Deploy to Local') {
            steps {
                sh 'docker run -d -p 8081:8080 demo-ci-cd:latest'
            }
        }
    }
}
