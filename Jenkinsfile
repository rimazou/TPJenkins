pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        bat 'gradle build'
        bat 'gradle javadoc'
        bat 'gradle jar'
        junit 'reports/**'
      }
    }

    stage('Mail notif') {
      steps {
        emailext(subject: 'Jenkins notif', body: 'build done', to: 'ir_zourane@esi.dz', from: 'ir_zourane@esi.dz')
      }
    }

  }
}