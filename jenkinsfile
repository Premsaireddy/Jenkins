pipeline {
    agent any

    environment {
        MAVEN_HOME = tool 'Maven 3.6.3' // Specify the Maven tool name configured in Jenkins
        AWS_CREDENTIALS = credentials('aws-credentials-id') // Replace with your Jenkins credential ID
        SONARQUBE_SERVER = 'SonarQube Server' // Replace with your SonarQube server name configured in Jenkins
    }

    stages {
        stage('Wait Before Starting') {
            steps {
                echo 'Waiting for 1 minute before starting the pipeline...'
                sleep time: 1, unit: 'MINUTES' // Add a 1-minute delay
            }
        }

        stage('Build') {
            steps {
                echo 'Building the project using Maven...'
                sh "${MAVEN_HOME}/bin/mvn clean package"
            }
        }

        stage('Unit and Integration Tests') {
            steps {
                echo 'Running Unit and Integration Tests...'
                sh "${MAVEN_HOME}/bin/mvn test"
                junit '**/target/surefire-reports/*.xml' // Collect JUnit test results
            }
        }

        stage('Code Analysis') {
            steps {
                echo 'Analyzing code using SonarQube...'
                withSonarQubeEnv(SONARQUBE_SERVER) {
                    sh "${MAVEN_HOME}/bin/mvn sonar:sonar"
                }
            }
        }

        stage('Security Scan') {
            steps {
                echo 'Running Security Scan using OWASP Dependency-Check...'
                sh 'dependency-check.sh --project your-project-name --scan .'
            }
        }

        stage('Deploy to Staging') {
            steps {
                echo 'Deploying to Staging environment...'
                withCredentials([string(credentialsId: 'AWS_ACCESS_KEY_ID', variable: 'AWS_ACCESS_KEY_ID'),
                                string(credentialsId: 'AWS_SECRET_ACCESS_KEY', variable: 'AWS_SECRET_ACCESS_KEY')]) {
                    sh '''
                        aws configure set aws_access_key_id $AWS_ACCESS_KEY_ID
                        aws configure set aws_secret_access_key $AWS_SECRET_ACCESS_KEY
                        aws ec2 describe-instances --region your-region --instance-ids i-xxxxxxxxxxxxx
                        # Add your deployment commands here
                    '''
                }
            }
        }

        stage('Integration Tests on Staging') {
            steps {
                echo 'Running Integration Tests on Staging environment...'
                sh 'run-staging-integration-tests.sh' // Replace with your test script or commands
            }
        }

        stage('Deploy to Production') {
            steps {
                input message: 'Ready to deploy to production?', ok: 'Deploy'
                echo 'Deploying to Production environment...'
                withCredentials([string(credentialsId: 'AWS_ACCESS_KEY_ID', variable: 'AWS_ACCESS_KEY_ID'),
                                string(credentialsId: 'AWS_SECRET_ACCESS_KEY', variable: 'AWS_SECRET_ACCESS_KEY')]) {
                    sh '''
                        aws configure set aws_access_key_id $AWS_ACCESS_KEY_ID
                        aws configure set aws_secret_access_key $AWS_SECRET_ACCESS_KEY
                        aws ec2 describe-instances --region your-region --instance-ids i-xxxxxxxxxxxxx
                        # Add your deployment commands here
                    '''
                }
            }
        }
    }

    post {
        always {
            echo 'Cleaning up...'
            cleanWs()
        }
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check logs for details.'
        }
    }
}
