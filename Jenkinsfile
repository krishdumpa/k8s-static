pipeline {
  agent any
  stages {
    stage('checkout') {
      steps {
        git(url: 'git@github.com:krishdumpa/k8s-static.git', branch: 'main')
      }
    }

    stage('build') {
      steps {
        sh 'sudo docker build -t 862982157705.dkr.ecr.us-east-2.amazonaws.com/k8s-static:$BUILD_NUMBER .'
      }
    }

    stage('longin_ECR') {
      steps {
        sh 'aws ecr get-login-password --region us-east-2 | sudo docker login --username AWS --password-stdin 862982157705.dkr.ecr.us-east-2.amazonaws.com'
      }
    }

    stage('upload') {
      steps {
        sh 'sudo docker push 862982157705.dkr.ecr.us-east-2.amazonaws.com/k8s-static:$BUILD_NUMBER'
      }
    }

    stage('deploy') {
      steps {
        sh '''sed -i s/number/$BUILD_NUMBER/g k8s/deployment.yaml
kubectl apply f k8s/'''
      }
    }

  }
}