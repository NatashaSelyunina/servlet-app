pipeline {
    agent any

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
                sshagent(['Jenkins-SSH-to-Tomcat']) {
                    sh """
                        scp -o StrictHostKeyChecking=no target/*.war root@94.103.12.198:~/deploy/
                        ssh root@94.103.12.198 'mv ~/deploy/*.war /opt/tomcat/webapps/app.war && systemctl restart tomcat'
                    """
                }
            }
        }
    }
}
