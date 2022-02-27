pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        withGradle()
      }
    }

    stage('Mail notification') {
      steps {
        mail(subject: 'jenkins', body: 'mail from jenkins')
      }
    }

    stage('Slack notification') {
      steps {
        slackSend(message: 'notif jenkins')
      }
    }

  }
}