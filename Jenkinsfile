pipeline {
  agent any
  stages {
    stage('Checkout') {
      steps { checkout scm }
    }
    stage('Build & Test') {
      steps {
        script {
          if (isUnix()) {
            sh 'mvn -B clean test'
            sh 'mvn -B package'
          } else {
            bat 'mvn -B clean test'
            bat 'mvn -B package'
          }
        }
      }
    }
    stage('Archive') {
      steps {
        junit 'target/surefire-reports/*.xml'
        archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
      }
    }
  }
}
