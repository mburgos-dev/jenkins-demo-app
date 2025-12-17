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
          . venv/bin/activate
          export PYTHONPATH=$PYTHONPATH:$(pwd)
          pytest
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
