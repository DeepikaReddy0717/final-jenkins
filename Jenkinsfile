pipeline {
    agent any

    stages {

        // ===== FRONTEND BUILD =====
        stage('Build Frontend') {
            steps {
                dir('STUDENTAPI-REACT') {
                    bat 'npm install'
                    bat 'npm run build'
                }
            }
        }

        // ===== FRONTEND DEPLOY =====
        stage('Deploy Frontend to Tomcat') {
            steps {
                bat '''
                if exist "C:\\Program Files\\Apache Software Foundation\\Tomcat 10.1\\webapps\\final-f1" (
                    rmdir /S /Q "C:\\Program Files\\Apache Software Foundation\\Tomcat 10.1\\webapps\\final-f1"
                )

                mkdir "C:\\Program Files\\Apache Software Foundation\\Tomcat 10.1\\webapps\\final-f1"
                xcopy /E /I /Y STUDENTAPI-REACT\\dist\\* "C:\\Program Files\\Apache Software Foundation\\Tomcat 10.1\\webapps\\final-f1"

                '''
            }
        }

        // ===== BACKEND BUILD =====
        stage('Build Backend') {
            steps {
                dir('STUDENTAPI-SPRINGBOOT') {
                    bat 'mvn clean package -DskipTests'
                }
            }
        }

        // ===== BACKEND DEPLOY =====
        stage('Deploy Backend to Tomcat') {
    steps {
        bat '''
        if exist "C:\\Program Files\\Apache Software Foundation\\Tomcat 10.1\\webapps\\final-f2.war" (
            del /Q "C:\\Program Files\\Apache Software Foundation\\Tomcat 10.1\\webapps\\final-f2.war"
        )
        if exist "C:\\Program Files\\Apache Software Foundation\\Tomcat 10.1\\webapps\\final-f2" (
            rmdir /S /Q "C:\\Program Files\\Apache Software Foundation\\Tomcat 10.1\\webapps\\final-f2"
        )
        copy "STUDENTAPI-SPRINGBOOT\\target\\final-f2.war" "C:\\Program Files\\Apache Software Foundation\\Tomcat 10.1\\webapps\\final-f2.war"
        '''
    }
}

    }

    post {
        success {
            echo ' Deployment Successful!'
        }
        failure {
            echo ' Pipeline Failed. Check Jenkins logs.'
        }
    }
}