pipeline {
  agent any
  stages {
    stage('Checkout') {
      steps { checkout scm }
    }
    stage('Lint Manifests') {
      steps { sh 'kubectl apply --dry-run=client --validate=false -f k8s/' }
    }
    stage('Cluster Check') {
      steps { sh 'kubectl get nodes' }
    }
    stage('Deploy') {
      steps { sh 'kubectl apply -f k8s/' }
    }
    stage('Verify Rollout') {
      steps {
        sh '''
          kubectl rollout status deployment/cloudkart-backend --timeout=120s
          kubectl rollout status deployment/cloudkart-frontend --timeout=120s
          kubectl get pods,svc
        '''
      }
    }
  }
}