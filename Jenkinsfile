pipeline {
  agent any
environment {
    PATH = "C:\\Program Files\\Git\\usr\\bin;C:\\Program Files\\Git\\bin;${env.PATH}"
  }
  tools {
    maven 'localMaven'
    jdk 'localJdk'
  }
  stages {
    stage('Build') {
      steps {
        bat 'mvn clean package'
      }
      post {
        success {
          echo ' now Archiving '
          archiveArtifacts artifacts: '**/*.war'
        }
      }
    }
    stage('SonarQube Scan') {
<<<<<<< HEAD
      mvn clean verify sonar:sonar \
        -Dsonar.projectKey=sonart \
        -Dsonar.host.url=http://localhost:9000 \
        -Dsonar.login=5fb25891fe10c975b429dafc5ba9cf342f79ce67
=======
      steps {
        sh """mvn sonar:sonar \
  -Dsonar.host.url=http://35.174.8.180:9000 \
  -Dsonar.login=e080ed1c637c253785555b7aad92969c50e8e820"""
      }
>>>>>>> parent of 1982a57 (Update Jenkinsfile)
    }
    stage('Upload to Artifactory') {
      steps {
        sh "mvn clean deploy -DskipTests"
      }

    }
    stage('Deploy to DEV') {
      environment {
        HOSTS = "dev"
      }
      steps {
        sh "ansible-playbook ${WORKSPACE}/deploy.yaml --extra-vars \"hosts=$HOSTS workspace_path=$WORKSPACE\""
      }

    }
    stage('Approval') {
      steps {
        input('Do you want to proceed?')
      }
    }
    stage('Deploy to PROD') {
      environment {
        HOSTS = "prod"
      }
      steps {
        sh "ansible-playbook ${WORKSPACE}/deploy.yaml --extra-vars \"hosts=$HOSTS workspace_path=$WORKSPACE\""
      }
    }
  }

}
