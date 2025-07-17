pipeline {
    agent {
        kubernetes {
            label 'jenkins-slave-agent'
        }
    }
    stages {
        stage('SCM') {
            checkout scm
        }
        stage('SonarQube Analysis') {
            def scannerHome = tool 'sonar-server';
            withSonarQubeEnv() {
            sh "${scannerHome}/bin/sonar-scanner"
            }
        }
    }
  
}
}

