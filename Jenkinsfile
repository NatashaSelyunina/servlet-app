pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Deploy') {
            environment {
                TOMCAT_HOME = '/opt/tomcat'
                WAR_FILE = 'target/servlet_1.0-1.0-SNAPSHOT.war'
            }

            steps {
                sh "${TOMCAT_HOME}/bin/shutdown.sh"
                sh "rm -rf ${TOMCAT_HOME}/webapps/servlet_1.0-1.0-SNAPSHOT*"
                sh "cp ${WAR_FILE} ${TOMCAT_HOME}/webapps/app.war"
                sh "${TOMCAT_HOME}/bin/startup.sh"
            }
        }

        stage('Verify') {
            steps {
                sh "sleep 30"
                sh "curl -s http://94.103.12.198:8085/hello"
            }
        }
    }

     post {
        always {
          sh "rm -rf ${WAR_FILE}"
        }
     }
}
