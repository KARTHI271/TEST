pipeline {
  agent any

  stages {
    stage('Checkout') {
      steps { checkout scm }
    }

    stage('Make script executable') {
      steps { sh 'if [ -f build.sh ]; then chmod +x build.sh; fi' }
    }

    stage('Run build script') {
      steps { sh './build.sh' }
    }

    stage('Collect artifacts') {
      steps { sh 'ls -lah || true' }
    }
  }

  post {
    success {
      archiveArtifacts artifacts: '**/*.txt, **/*.log', allowEmptyArchive: true
      emailext(
        to: 'karthisv2701@gmail.com',
        subject: "SUCCESS: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
        body: """Build SUCCESS

Job: ${env.JOB_NAME}
Build: ${env.BUILD_NUMBER}
URL: ${env.BUILD_URL}

---- last 200 lines of console ----
${BUILD_LOG, maxLines=200}
"""
      )
    }

    failure {
      archiveArtifacts artifacts: '**/*.txt, **/*.log', allowEmptyArchive: true
      emailext(
        to: 'karthisv2701@gmail.com',
        subject: "FAILURE: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
        body: """Build FAILED

Job: ${env.JOB_NAME}
Build: ${env.BUILD_NUMBER}
URL: ${env.BUILD_URL}

---- last 200 lines of console ----
${BUILD_LOG, maxLines=200}
"""
      )
    }
  }
}
