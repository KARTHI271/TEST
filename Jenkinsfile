pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Make Script Executable') {
            steps {
                sh 'chmod +x build.sh'
            }
        }

        stage('Run Build Script') {
            steps {
                sh './build.sh'
            }
        }
    }

    post {
        success {
            emailext(
                to: 'karthisv2701@gmail.com',
                subject: "SUCCESS: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: """✔ BUILD SUCCESS

JOB: ${env.JOB_NAME}
BUILD NUMBER: ${env.BUILD_NUMBER}
BUILD URL: ${env.BUILD_URL}

---- LAST 200 LINES OF LOG ----
${BUILD_LOG, maxLines=200}
"""
            )
        }

        failure {
            emailext(
                to: 'karthisv2701@gmail.com',
                subject: "FAILURE: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: """❌ BUILD FAILED

JOB: ${env.JOB_NAME}
BUILD NUMBER: ${env.BUILD_NUMBER}
BUILD URL: ${env.BUILD_URL}

---- LAST 200 LINES OF LOG ----
${BUILD_LOG, maxLines=200}
"""
            )
        }
    }
}
