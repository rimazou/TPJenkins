pipeline {
  agent any
  stages {
    stage('Build') {
      post {
        success {
          mail(subject: 'Build success', body: 'Build completed successfully', from: 'ir_zourane@esi.dz', to: 'ir_zourane@esi.dz')
        }

        failure {
          mail(subject: 'Build failed', body: 'Something went wrong with the build', from: 'ir_zourane@esi.dz', to: 'ir_zourane@esi.dz')
        }

      }
      steps {
        bat 'gradle build'
        bat 'gradle javadoc'
        bat 'gradle jar'
        bat 'gradle jacocoTestReport'
        archiveArtifacts 'build/libs/*.jar'
      }
    }

    stage('Mail notif') {
      steps {
        mail(subject: 'Build Complete', body: 'Build completed', from: 'ir_zourane@esi.dz', to: 'ir_zourane@esi.dz')
      }
    }

    stage('Code Analysis') {
      parallel {
        stage('Code Analysis') {
          post {
            failure {
              mail(subject: 'sonar failed', body: 'Something went wrong with the scan', from: 'ir_zourane@esi.dz', to: 'ir_zourane@esi.dz')
            }

          }
          steps {
            bat 'gradle sonarqube'
          }
        }

        stage('Test Reporting') {
          steps {
            cucumber(fileIncludePattern: '**/Cucumber.json', buildStatus: 'Unstable', jsonReportDirectory: 'C:\\ProgramData\\Jenkins\\.jenkins\\workspace\\TPJenkins_main\\build\\reports')
          }
        }

      }
    }

    stage('Deployment') {
      steps {
        bat 'gradle jar'
        bat 'gradle publish'
      }
    }

    stage('Slack notif') {
      steps {
        slackSend(attachments: 'Deploy', baseUrl: 'https://hooks.slack.com/services', token: 'T02TY4XHSTA/B0350LLFUQ2/Vg7GvzlptiNS4qqcDvTvvMHm', tokenCredentialId: 'xoxe.xoxp-1-Mi0yLTI5NTAxNjc2MDQ5MjgtMjkyODczMDc1NTg0NC0zMTgzNjM1NTU5NDI1LTMxNzMzNTIxNzUzMDAtODRjNTU5ZTBiZTM2NjFkMGM0OWQ1OWU0NGE0NTA0ZDcwNTBiYzhlZTc2ZGY4OTdiMTIwZGIwYjRkMDc4ZTI4OA')
      }
    }

  }
}