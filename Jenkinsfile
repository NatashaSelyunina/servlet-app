pipeline {
    agent any

    environment {
        DEPLOY_HOST = 's402746.foxcdn.ru'
        DEPLOY_USER = 'root'
        DEPLOY_PATH = '/opt/tomcat/webapps'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    credentialsId: 'GitHub-Jenkins-SSH',
                    url: 'git@github.com:NatashaSelyunina/servlet-app.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Deploy') {
            steps {
                script {
                    sh "scp target/servlet_1.0-1.0-SNAPSHOT.war ${DEPLOY_USER}@${DEPLOY_HOST}:${DEPLOY_PATH}/"
                    sh "ssh ${DEPLOY_USER}@${DEPLOY_HOST} 'sudo systemctl restart tomcat'"
                }
            }
        }
    }
}
