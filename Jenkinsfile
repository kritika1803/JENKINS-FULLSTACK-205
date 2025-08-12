pipeline {
    agent any

    stages {

        // ===== FRONTEND BUILD =====
        stage('Build Frontend') {
            steps {
                dir('STUDENTAPI-REACT') {
                    sh '''
                    npm install
                    npm run build
                    '''
                }
            }
        }

        // ===== FRONTEND DEPLOY =====
        stage('Deploy Frontend to Tomcat') {
            steps {
                sh '''
                TOMCAT_WEBAPPS="/Users/chilakakritikareddy/Desktop/SOFTWARE/apache-tomcat-10.1.43/webapps"

                rm -rf "$TOMCAT_WEBAPPS/reactstudentapi"
                mkdir -p "$TOMCAT_WEBAPPS/reactstudentapi"
                cp -R STUDENTAPI-REACT/dist/* "$TOMCAT_WEBAPPS/reactstudentapi"
                '''
            }
        }

        // ===== BACKEND BUILD =====
        stage('Build Backend') {
            steps {
                dir('STUDENTAPI-SPRINGBOOT') {
                    sh '''
                    mvn clean package
                    '''
                }
            }
        }

        // ===== BACKEND DEPLOY =====
        stage('Deploy Backend to Tomcat') {
            steps {
                sh '''
                TOMCAT_WEBAPPS="/Users/chilakakritikareddy/Desktop/SOFTWARE/apache-tomcat-10.1.43/webapps"

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
