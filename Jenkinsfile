pipeline {
    agent any

    environment {
        PATH = "/usr/local/bin:${env.PATH}" // ensure npm & node are visible
    }

    stages {

        // ===== FRONTEND BUILD =====
        stage('Build Frontend') {
            steps {
                dir('STUDENTAPI-REACT') {
                    sh 'npm install'
                    sh 'npm run build'
                }
            }
        }

        // ===== FRONTEND DEPLOY =====
        stage('Deploy Frontend to Tomcat') {
            steps {
                sh '''
                TOMCAT_WEBAPPS="/Users/chilakakritikareddy/Desktop/SOFTWARE/apache-tomcat-10.1.XX/webapps/reactstudentapi"
                rm -rf "$TOMCAT_WEBAPPS"
                mkdir -p "$TOMCAT_WEBAPPS"
                cp -R STUDENTAPI-REACT/dist/* "$TOMCAT_WEBAPPS"
                '''
            }
        }

        // ===== BACKEND BUILD =====
        stage('Build Backend') {
            steps {
                dir('STUDENTAPI-SPRINGBOOT') {
                    sh '/Users/chilakakritikareddy/Desktop/SOFTWARE/apache-maven-3.9.11/bin/mvn clean package'
                }
            }
        }

        // ===== BACKEND DEPLOY =====
        stage('Deploy Backend to Tomcat') {
            steps {
                sh '''
                TOMCAT_WEBAPPS="/Users/chilakakritikareddy/Desktop/SOFTWARE/apache-tomcat-10.1.XX/webapps"
                rm -f "$TOMCAT_WEBAPPS/springbootstudentapi.war"
                rm -rf "$TOMCAT_WEBAPPS/springbootstudentapi"
                cp STUDENTAPI-SPRINGBOOT/target/*.war "$TOMCAT_WEBAPPS/"
                '''
            }
        }

    }

    post {
        success {
            echo 'Deployment Successful!'
        }
        failure {
            echo 'Pipeline Failed.'
        }
    }
}
