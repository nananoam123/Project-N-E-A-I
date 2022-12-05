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
      agent {
    label 'win'
  }
      steps {
        sh 'terraform apply -auto-approve'
      }
    }

    stage('Copy kubeconfig') {
      steps {
        sh 'aws eks --region ap-northeast-1 update-kubeconfig --name Project-E-N-A-I-eks'
      }
    }

  }
}
