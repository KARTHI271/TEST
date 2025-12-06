pipeline {
  agent any

  options {
    buildDiscarder(logRotator(numToKeepStr: '30'))
    timestamps()
  }

  stages {
    stage('Checkout') {
      steps { checkout scm }
    }

    stage('Prepare') {
      steps {
        script {
          if (!fileExists('build.sh')) {
            error "build.sh not found in repo root!"
          }
          sh 'chmod +x build.sh'
        }
      }
    }

    stage('Run build') {
      steps {
        sh './build.sh'
      }
    }

    stage('Workspace list') {
      steps {
        sh 'echo "WORKSPACE: $(pwd)"; ls -lah'
      }
    }
  }

  post {
    success {
      archiveArtifacts artifacts: '**/*.txt, **/*.log', allowEmptyArchive: true
      emailext(
        to: 'karthisv2701@gmail.com',
        from: 'jenkins@your-verified-domain.com',
        subject: "SUCCESS: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
        body: """Build SUCCESS

Job: ${env.JOB_NAME}
Build Number: ${env.BUILD_NUMBER}
URL: ${env.BUILD_URL}

---- last 200 lines ----
${BUILD_LOG, maxLines=200}
"""
      )
    }
    failure {
      archiveArtifacts artifacts: '**/*.txt, **/*.log', allowEmptyArchive: true
      emailext(
        to: 'karthisv2701@gmail.com',
        from: 'jenkins@your-verified-domain.com',
        subject: "FAILURE: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
        body: """Build FAILED

Job: ${env.JOB_NAME}
Build Number: ${env.BUILD_NUMBER}
URL: ${env.BUILD_URL}

---- last 200 lines ----
${BUILD_LOG, maxLines=200}
"""
      )
    }
    always {
      echo "Pipeline finished with status: ${currentBuild.currentResult}"
    }
  }
}
