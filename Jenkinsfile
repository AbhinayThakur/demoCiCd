pipeline {
    agent any

    tools {
        maven 'Maven3'
        jdk 'Java17'
    }

    stages {
        stage('Checkout') {
            steps {
                // Using local Git repository
                checkout([
                    $class: 'GitSCM',
                    branches: [[name: '*/main']],
                    extensions: [],
                    userRemoteConfigs: [[url: '.']]
                ])
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
                script {
                    docker.build("demo-ci-cd:${env.BUILD_NUMBER}")
                }
            }
        }
        stage('Deploy to Local') {
            steps {
                sh 'docker stop demo-ci-cd || true'
                sh 'docker rm demo-ci-demo || true'
                sh "docker run -d -p 8081:8080 --name demo-ci-cd demo-ci-cd:${env.BUILD_NUMBER}"
            }
        }
    }
}
