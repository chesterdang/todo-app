pipeline {
    agent {
        kubernetes {
            yaml '''
apiVersion: v1
kind: Pod
metadata:
labels:
  project-name: demo
spec:
  serviceAccountName: k8s-builder
  containers:
    - name: docker
      image: docker:23.0.4-cli
      tty: true
    - name: helm
      image: alpine/helm:3.12.0
      command:
        - /bin/cat
      tty: true
    - name: gh
      image: maniator/gh:v2.29.0
      command:
        - /bin/cat
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

