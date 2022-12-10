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
        sh 'terraform init'
      }
    }

    stage('terraform apply') {
      steps {
        sh ' terraform apply -auto-approve'
      }
    }

    stage('Copy kubeconfig') {
      steps {
        node(label: 'win') {
          sh ''' rm -rf /c/jenkins/workspace/Project-N-E-A-I_main/Hello-world-deployment
aws eks --region ap-northeast-1 update-kubeconfig --name Project-E-N-A-I-eks'''
        }

      }
    }

    stage('helm install') {
      steps {
        sh '''kubectl create namespace monitoring
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update 
helm upgrade --namespace monitoring --install kube-stack-prometheus prometheus-community/kube-prometheus-stack --set prometheus-node-exporter.hostRootFsMount.enabled=false'''
      }
    }

    stage('Expose services ') {
      steps {
        node(label: 'win') {
          sh '''JENKINS_NODE_COOKIE=dontKillMe 
kubectl port-forward --namespace monitoring svc/kube-stack-prometheus-kube-prometheus 9090:9090 & 
JENKINS_NODE_COOKIE=dontKillMe 
kubectl port-forward --namespace monitoring svc/kube-stack-prometheus-grafana 7000:80 & '''
        }

      }
    }

    stage('Deployment') {
      steps {
        node(label: 'win') {
          sh 'git clone https://github.com/nananoam123/Hello-world-deployment.git'
          dir(path: 'Hello-world-deployment') {
            sh 'kubectl create -f deployment.yaml'
          }

          sh 'kubectl expose deployment/projecthelloworld'
        }

      }
    }

    stage('Expose deployment') {
      steps {
        node(label: 'win') {
          sh '''sleep 4
JENKINS_NODE_COOKIE=dontKillMe
kubectl port-forward svc/projecthelloworld 8000:8080 &'''
        }

      }
    }

  }
}