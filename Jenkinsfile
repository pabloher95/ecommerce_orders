@Library('ecommerce-shared-library') _

pipeline {
  agent any
  tools {
    nodejs 'NodeJS'
  }
  environment {
    PATH = "/usr/local/bin:${env.PATH}"
    REGISTRY = 'pabloher95'
    IMAGE_NAME = 'ecommerce-order-service'
  }
  stages {
    stage('Build') {
      when { anyOf { branch 'develop'; branch 'main'; changeRequest() } }
      steps { buildNode() }
    }
    stage('Test') {
      when { anyOf { branch 'develop'; branch 'main'; changeRequest() } }
      steps { testNode() }
    }
    stage('Container Build') {
      when { anyOf { branch 'develop'; branch 'main'; branch pattern: 'release/.*', comparator: 'REGEXP' } }
      steps { containerBuild(service: 'order-service') }
    }
    stage('Security Scan') {
      when { anyOf { branch 'develop'; branch 'main'; changeRequest() } }
      steps { securityScan(service: 'order-service') }
    }
    stage('Container Push') {
      when { anyOf { branch 'develop'; branch 'main'; branch pattern: 'release/.*', comparator: 'REGEXP' } }
      steps { containerPush(service: 'order-service') }
    }
    stage('Deploy') {
      when {
        anyOf {
          branch 'develop'
          branch pattern: 'release/.*', comparator: 'REGEXP'
          branch 'main'
        }
      }
      steps { deploy(service: 'order-service') }
    }
  }
}