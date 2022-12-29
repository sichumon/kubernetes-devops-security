@Library('slack') _

pipeline {
  agent any

  environment {
    deploymentName = "devsecops"
    containerName = "devsecops-container"
    serviceName = "devsecops-svc"
    imageName = "siddharth67/numeric-app:${GIT_COMMIT}"
    applicationURL = "http://devsecops-demo.eastus.cloudapp.azure.com"
    applicationURI = "/increment/99"
  }

  stages {
    stage('Testing Slack') {
      steps {
        sh 'exit 1'
      }
    }

  }

  post {
    always {
      // junit 'target/surefire-reports/*.xml'
      // jacoco execPattern: 'target/jacoco.exec'
      // pitmutation mutationStatsFile: '**/target/pit-reports/**/mutations.xml'
      // dependencyCheckPublisher pattern: 'target/dependency-check-report.xml'
      // publishHTML([allowMissing: false, alwaysLinkToLastBuild: true, keepAll: true, reportDir: 'owasp-zap-report', reportFiles: 'zap_report.html', reportName: 'OWASP ZAP HTML Report', reportTitles: 'OWASP ZAP HTML Report'])

      

      // Use sendNotifications.groovy from shared library and provide current build result as parameter    
      sendNotification currentBuild.result
    }

    // success {

    // }

    // failure {

    // }
  }

}