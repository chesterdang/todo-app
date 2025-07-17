pipeline {
    agent {
        kubernetes {
            yaml '''
                apiVersion: V1
                kind: Pod
                spec: 
                    containers:
                    - name: maven
                      image: maven:alpine
                      command: 
                      - cat
                      tty: true
            '''
        }
    }
    stages {
        stage('SCM') {
            steps {
                checkout scm
            }
        }
        stage('SonarQube Analysis') {
            steps {
                script {
                    def scannerHome = tool 'sonar-server'
                    withSonarQubeEnv() {
                        sh "${scannerHome}/bin/sonar-scanner"
                    }
                }
            }
        }
        stage('Build Backend Docker Image') {
            steps {
                dir('backend') {
                    script {
                        docker.build('chesterdang/todo-app-backend:latest', '.')
                    }
                }
            }
        }
        stage('Build Frontend Docker Image') {
            steps {
                dir('frontend') {
                    script {
                        docker.build('chesterdang/todo-app-frontend:latest', '.')
                    }
                }
            }
        }
        // Add push or deploy stages as needed
    }
}

