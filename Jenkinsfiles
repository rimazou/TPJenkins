pipeline {
  agent any
  stages {
    stage('Build') {
      post {
        failure {
          mail(subject: 'Build DONE', body: 'The build has not been succesful', to: 'ir_zourane@esi.dz')

        }

        success {
          mail(subject: 'Build DONE', body: 'The build has been succesfully done', to: 'ir_zourane@esi.dz')

        }

      }
      steps {
        sh 'gradle build'
        sh 'gradle jar'
        sh 'gradle javadoc'
        archiveArtifacts(onlyIfSuccessful: true, artifacts: 'build/libs/*.jar')
      }
    }
    stage('Code Analysis') {
      parallel {
        stage('Code Analysis') {
          steps {
            script {
              scannerHome = tool 'SonarQubeScanner'
            }

            withSonarQubeEnv('sonarqube') {
              sh 'C:\\\\sonarqube-7.4\\\\bin\\\\windows-x86-64sonar-scanner.bat'
            }

          }
        }
        stage('Test Reporting') {
          steps {
            sh 'gradle test'
            jacoco(buildOverBuild: true)
          }
        }
      }
    }
    stage('Deployement') {
      when{
            not{
              changeRequest target : 'master'
            }
          }
      steps {
        sh 'gradle uploadArchives'
      }
    }
    stage('Slack Notifcations') {
      
      when{
            not{
              changeRequest target : 'master'
            }
          }
      steps {
        slackSend(failOnError: true, message: 'the uploading is done succefully')
      }
    }
  }
}
