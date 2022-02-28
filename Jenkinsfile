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
        mail(subject: 'Build Complete', body: 'Build', from: 'ir_zourane@esi.dz', to: 'ir_zourane@esi.dz')
      }
    }

    stage('test') {
      steps {
        bat 'gradle test'
      }
    }

    stage('documentation') {
      parallel {
        stage('Code Analysis') {
          steps {
            waitForQualityGate true
          }
        }

        stage('Cucumber') {
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

    stage('Slack Notif') {
      steps {
        slackSend(baseUrl: 'https://hooks.slack.com/services/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', token: 'xapp-1-A02T1NA6Z9U-3154115032311-f98f2aea5e021c531987a31803e06dc232d17ae65f15b8c0e58b7fbca7763940', message: 'slack notification', blocks: 'this is a notif from jenkins', attachments: 'Notif Jenkins', channel: 'dev', color: 'green', sendAsText: true, username: 'ir_zourane')
      }
    }

  }
}