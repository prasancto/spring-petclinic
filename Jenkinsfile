pipeline {
    stages {
        stage ('Checkout') {
          steps {
            git 'https://github.com/prasancto/spring-petclinic.git'
          }
        }
        stage('Build') {
            agent { docker 'maven:3.5-alpine' }
            steps {
                sh 'mvn clean package'
                junit '**/target/surefire-reports/TEST-*.xml'
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }
        stage('Deploy') {
          steps {
            input 'Do you approve the deployment?'
            sh 'scp target/*.jar jenkins@10.150.231.6:/pkg/pet/'
            sh "ssh jenkins@10.150.231.6 'nohup java -jar /opt/pet/spring-petclinic-1.5.1.jar &'"
          }
       }
    }
  }
