pipeline {
    agent any

    tools {
        maven 'Maven_3.8' 
    }

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/rdjastony/Eccomerce', branch: 'main'
            }
        }

        stage('Build with Maven') {
            steps {
                sh 'mvn clean install'
            }
        }
    }
}
