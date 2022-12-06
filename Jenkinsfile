pipeline {
  agent {label 'terraform'} 
  stages {
    stage('Checkout code') {
      steps {
        cleanWs()
        git(url: 'https://github.com/nananoam123/Project-N-E-A-I.git', branch: 'main', credentialsId: 'github')
      }
    }

    stage('terraform init') {
      steps {
        sh 'echo hello world '
      }
    }

    stage('terraform apply') {
      steps {
        sh 'echo hello world'
      }
    }

    stage('Copy kubeconfig') {
      steps {
        node('Built-In Node'){
        
        sh 'aws eks --region ap-northeast-1 update-kubeconfig --name Project-E-N-A-I-eks'
      }
    }
   }

    stage('helm install') {
      steps {
        sh '''kubectl create namespace monitoring
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
helm upgrade --namespace monitoring --install kube-stack-prometheus prometheus-community/kube-prometheus-stack --set prometheus-node-exporter.hostRootFsMount.enabled=false
'''
      }
    }

  }
}
