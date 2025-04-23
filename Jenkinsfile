pipeline {
  agent { label 'dind-agent' }

  parameters {
    booleanParam(name: 'DEPLOY', defaultValue: false, description: 'Déployer l’API ?')
  }

  stages {
    stage('Checkout') {
      steps {
        git url: 'https://github.com/Talsadoom51/chuck_citations-api.git', branch: 'main'
        sh 'ls -la'
      }
    }

    stage('Build Docker image') {
      steps {
        sh '''
        docker build -t mon-registry/chuck-api:latest .
        docker push mon-registry/chuck-api:latest
        '''
      }
    }

    stage('Deploy') {
      when { expression { return params.DEPLOY } }
      steps {
        sh '''
        ssh vagrant@prod "docker pull mon-registry/chuck-api:latest && docker restart chuck_api"
        '''
      }
    }
  }
}
