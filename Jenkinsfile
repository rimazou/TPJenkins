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

  }
}