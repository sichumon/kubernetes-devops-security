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
      stage('Mutation Tests - PIT') {
        steps {
          sh "mvn org.pitest:pitest-maven:mutationCoverage"
        }
        post {
          always {
            pitmutation mutationStatsFile: '**/target/pit-reports/**/mutations.xml'
          }
        }
      }
     
      // stage('SonarQube - SAST') {
      //   steps {
      //     withSonarQubeEnv('SonarQube') {
      //       sh "mvn sonar:sonar -Dsonar.projectKey=numeric-application -Dsonar.host.url=http://greenops.uksouth.cloudapp.azure.com:9000 -Dsonar.login=50222058b2f3241602e26fe01d98f282a65e4c01" 
      //     }
      //     timeout(time: 2, unit: 'MINUTES') {
      //       script {
      //         waitForQualityGate abortPipeline: true
      //       }
      //     }
      //   }
      // }
      
      stage('Vulnerability Scan - Docker ') {
            steps {
              sh "mvn dependency-check:check"
            }
            post {
              always {
                dependencyCheckPublisher pattern: 'target/dependency-check-report.xml'
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
      stage('Kubernetes Deployment - DEV') {
        steps {
          withKubeConfig([credentialsId: 'kubeconfig']) {
            sh "sed -i 's#replace#jstan77/numeric-app:${GIT_COMMIT}#g' k8s_deployment_service.yaml"
            sh "kubectl apply -f k8s_deployment_service.yaml"
        }
      }
    }
    }
}


