pipeline {
  //Esto significa que lo ejecutará cualquier agente
  agent any

  stages {
    stage('Checkout') {
      steps {
        //Clona el repo donde esta el Jenkinsfile
        checkout scm
      }
    }

    stage('Install dependencies') {
      steps {
        sh '''
          python3 -m venv venv
          . venv/bin/activate
          pip install -r requirements.txt
        '''
      }
    }

    stage('Run tests') {
      steps {
        sh '''
          . venv/bin/activate
          pytest
        '''
      }
    }
  }
}
