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
                body: """Build SUCCESS

Job: ${env.JOB_NAME}
Build Number: ${env.BUILD_NUMBER}
Build URL: ${env.BUILD_URL}

Pipeline finished successfully.
"""
            )
        }

        failure {
            emailext(
                to: 'karthisv2701@gmail.com',
                subject: "FAILURE: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: """Build FAILED

Job: ${env.JOB_NAME}
Build Number: ${env.BUILD_NUMBER}
Build URL: ${env.BUILD_URL}

Pipeline failed.
"""
            )
        }
    }
}
