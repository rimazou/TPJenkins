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

    stage('Code Analysis') {
      parallel {
        stage('Code Analysis') {
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

    stage('Slack Notif') {
      steps {
        slackSend(baseUrl: 'https://hooks.slack.com/services/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', message: 'slack notification', blocks: 'this is a notif from jenkins', attachments: 'Notif Jenkins', channel: 'dev')
      }
    }

  }
}