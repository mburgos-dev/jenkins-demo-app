pipeline {
  agent any

  environment {
    IMAGE_NAME = "localhost:5000/jenkins-demo-app"
    IMAGE_TAG  = "latest"
  }

  stages {
    stage('Checkout') {
      steps {
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
          export PYTHONPATH=$PYTHONPATH:$(pwd)
          pytest
        '''
      }
    }

    stage('Build Docker image') {
      steps {
        sh '''
          docker build -t $IMAGE_NAME:$IMAGE_TAG .
        '''
      }
    }

    stage('Push Docker image') {
      steps {
        sh '''
          docker push $IMAGE_NAME:$IMAGE_TAG
        '''
      }
    }

    stage('Run Docker image') {
      steps {
        sh '''
          docker run --rm $IMAGE_NAME:$IMAGE_TAG
        '''
      }
    }
  }
}


