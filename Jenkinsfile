pipeline {
  agent any
  stages {
    stage('Build') {
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
        emailext(subject: 'Jenkins notif', body: 'build done', to: 'ir_zourane@esi.dz', from: 'ir_zourane@esi.dz')
      }
    }

    stage('build') {
      parallel {
        stage('build') {
          steps {
            bat 'gradle build'
            archiveArtifacts 'build/libs/*.jar'
          }
        }

        stage('mail') {
          steps {
            mail(subject: 'jenkins notif', body: 'jenkins build', cc: 'ir_zourane@esi.dz', from: 'ir_zourane@esi.dz')
          }
        }

      }
    }

    stage('test') {
      steps {
        bat 'gradle test'
      }
    }

    stage('documentation') {
      parallel {
        stage('documentation') {
          steps {
            bat 'gradle javadoc'
          }
        }

        stage('Cucumber') {
          steps {
            cucumber(fileIncludePattern: '**/Cucumber.json', buildStatus: 'Unstable', jsonReportDirectory: 'C:\\ProgramData\\Jenkins\\.jenkins\\workspace\\TPJenkins_main\\build\\reports')
          }
        }

      }
    }

    stage('deploy') {
      steps {
        bat 'gradle jar'
        bat 'gradle publish'
      }
    }

    stage('Slack') {
      steps {
        slackSend(baseUrl: 'https://hooks.slack.com/services/', token: 'xapp-1-A02T1NA6Z9U-3154115032311-f98f2aea5e021c531987a31803e06dc232d17ae65f15b8c0e58b7fbca7763940', message: 'slack notification')
      }
    }

  }
}