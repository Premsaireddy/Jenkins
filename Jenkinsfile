pipeline {
    agent any

    environment {
        STAGING_SERVER = 'user@staging-server' 
        SONARQUBE_SERVER = 'user@production-server' 
    }

    stages {
        stage('Build') {
            steps {
                echo 'Building the project using Maven...'
            }
        }

        stage('Unit and Integration Tests') {
            steps {
                echo 'Running Unit and Integration Tests...'
            }
            post{
                always {
                //script{
                //    emailext attachlog:true,
                //    to: "reddypremsai585@gmail.com",
                //    subject: "Test status Email: $(currentBuild.currentResult)",
                //    body: "Test completed with status: $(currentBuild.currentResult).",
                // )
                emailext(
                    attachLog: true,
                    to: "reddypremsai585@gmail.com",
                    subject: "Test status email: ${currentBuild.currentResult}",
                    body: "Test completed with status: ${currentBuild.currentResult}."
                )
                }
            }
        }
        stage('Code Analysis'){
            steps{
                echo "Analyzing the code quality for industry requirements"
            }
        }
        stage('Security Scan'){
            steps{
                echo "Performing the security scan on the code"
            }
            post{
                always {
                //script{
                //    emailext attachlog:true,
                //    to: "reddypremsai585@gmail.com",
                //    subject: "Test status Email: $(currentBuild.currentResult)",
                //    body: "Test completed with status: $(currentBuild.currentResult).",
                // )
                emailext(
                    attachLog: true,
                    to: "reddypremsai585@gmail.com",
                    subject: "Test status email: ${currentBuild.currentResult}",
                    body: "Test completed with status: ${currentBuild.currentResult}."
                )
                }
            }
        }
        stage('Deploy to staging'){
            steps{
                echo "Deploying the application to the staging server"
            }
        }
        stage("Integration tests on staging"){
            steps{
                echo "Running integration tests on staging using cucumber tests"
            }
        }
        stage("Deploy to production"){
            steps{
                echo "Deploying the application to the production server"
            }
        }
    }
    post{
        always{
            mail to: "reddypremsai585@gmail.com",
            subject: "Build status email: ${currentBuild.currentResult}",
            body: "Build completed with status: ${currentBuild.currentResult}."
        }
    }
}
        
