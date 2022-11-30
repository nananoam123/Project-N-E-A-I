pipeline {
  agent {
    label 'terraform'
  }
  stages {
    stage('Checkout code') {
      steps {
        cleanWs()
        git(url: 'https://github.com/nananoam123/Project-N-E-A-I.git', branch: 'main', credentialsId: 'github')
      }
    }

    stage('terraform init') {
      steps {
        sh 'terraform init '
      }
    }

    stage('terraform apply') {
      steps {
        sh 'terraform apply -auto-approve'
      }
    }

  }
}