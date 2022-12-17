pipeline {
  agent any

  stages {
      stage('Build Artifact') {
            steps {
              sh "mvn clean package -DskipTests=true"
              archive 'target/*.jar' 
            }
        }   
      stage('Unit test') {
            steps {
              sh "mvn test"
            }
            post {
              always {
                junit 'target/surefire-reports/*.xml'
                jacoco execPattern: 'target/jacoco.exec'
              }
            }
        }
      stage('Docker Build and Push') {
        steps {
            withDockerRegistry([credentialsId: "dockerhub", url: ""]) {
            sh 'printenv'
            sh 'docker build -t jstan77/numeric-app:""$GIT_COMMIT"" .'
            sh 'docker push jstan77/numeric-app:""$GIT_COMMIT""'
            }
        }
      }
    
    }
}
