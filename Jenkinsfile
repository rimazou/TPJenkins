pipeline {
  agent any
  stages {
    stage('Build') {
      post{
        success{mail(subject: 'Build success', body: 'Build completed successfully', from: 'ir_zourane@esi.dz', to: 'ir_zourane@esi.dz')}
        failure{mail(subject: 'Build failed', body: 'Something went wrong with the build', from: 'ir_zourane@esi.dz', to: 'ir_zourane@esi.dz')}
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
             post{
        failure{mail(subject: 'sonar failed', body: 'Something went wrong with the scan', from: 'ir_zourane@esi.dz', to: 'ir_zourane@esi.dz')}
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

    stage('Slack Notif') {
      steps {
        slackSend(baseUrl: 'https://hooks.slack.com/services/', message: 'slack notification', blocks: 'Deployement successful', attachments: 'Notif Jenkins', channel: 'dev', token: 'T02TY4XHSTA/B0350LLFUQ2/Vg7GvzlptiNS4qqcDvTvvMHm', username: 'ir_zourane')
      }
    }

  }
}
