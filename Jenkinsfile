pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        bat 'gradle build'
      }
    }

    stage('Mail notification') {
      steps {
        mail(subject: 'jenkins', body: 'mail from jenkins')
      }
    }

    stage('Slack notification') {
      steps {
        slackSend(message: 'notif jenkins', attachments: 'jenkins', blocks: 'hi its a notif from jenkins')
      }
    }

  }
}