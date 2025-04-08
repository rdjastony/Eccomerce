pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "abhishek7840/spring-boot-crud-example-master"
    }

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/rdjastony/Eccomerce', branch: 'main'
            }
        }

        stage('Build') {
            steps {
                sh './mvnw clean install' // uses wrapper if available
                // OR use 'sh 'mvn clean install'' if wrapper is not present
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${DOCKER_IMAGE} ."
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub-creds', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    sh 'echo $PASSWORD | docker login -u $USERNAME --password-stdin'
                    sh "docker push ${DOCKER_IMAGE}"
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f k8s/deployment.yaml'
                sh 'kubectl apply -f k8s/service.yaml'
            }
        }
    }
}
