pipeline {
  agent { label 'dind-agent' }

  parameters {
    booleanParam(name: 'DEPLOY', defaultValue: false, description: 'Déployer l’API ?')
  }

  stages {
    stage('Checkout') {
      steps {
        git url: 'https://github.com/Talsadoom51/chuck-citations-api.git', branch: 'main'
        sh 'ls -la'
      }
    }

    stage('Build Docker image') {
      steps {
        sh '''
        docker build -t 192.168.56.151:5000/chuck-api:latest .
        docker push 192.168.56.151:5000/chuck-api:latest
        '''
      }
    }

    stage('Deploy') {
      when { expression { return params.DEPLOY } }
      steps {
        sh '''
        ssh vagrant@192.168.56.152 "docker pull mon-registry/chuck-api:latest && docker restart chuck_api"
        '''
      }
    }
  }
}
