pipeline {
    agent any 

    stages {
        stage('Build') {
            steps {
                // Use Maven to build the project
                sh 'mvn clean install'
            }
        }
        stage('Unit and Integration Tests') {
            steps {
                // Use JUnit or TestNG for testing
                sh 'mvn test'
            }
        }
        stage('Code Analysis') {
            steps {
                // Use SonarQube for code analysis
                sh 'mvn sonar:sonar'
            }
        }
        stage('Security Scan') {
            steps {
                // Use OWASP Dependency-Check or similar tool
                sh 'dependency-check --project MyProject --scan ./'
            }
        }
        stage('Deploy to Staging') {
            steps {
                // Deploy to AWS EC2 staging server
                sh './deploy-to-staging.sh'
            }
        }
        stage('Integration Tests on Staging') {
            steps {
                // Use Postman/Newman for API integration testing
                sh 'newman run my-collection.postman_collection.json'
            }
        }
        stage('Deploy to Production') {
            steps {
                // Deploy to AWS EC2 production server
                sh './deploy-to-production.sh'
            }
        }
    }
    post {
        always {
            emailext (
                subject: "${currentBuild.result}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
                body: """<p>${currentBuild.result}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]':</p>
                         <p>Check console output at &QUOT;<a href='${env.BUILD_URL}'>${env.JOB_NAME} [${env.BUILD_NUMBER}]</a>&QUOT;</p>""",
                recipientProviders: [[$class: 'DevelopersRecipientProvider']],
                attachLog: true
            )
        }
    }
}
