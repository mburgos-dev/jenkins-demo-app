pipeline {
  agent any

  environment {
    IMAGE_NAME = "localhost:5000/jenkins-demo-app"
    IMAGE_TAG  = "${env.BRANCH_NAME}-${env.BUILD_NUMBER}"
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

    stage('Run tests (CI)') {
      steps {
        sh '''
          . venv/bin/activate
          export PYTHONPATH=$PYTHONPATH:$(pwd)
          pytest
        '''
      }
    }

    stage('Build image (CI)') {
      steps {
        sh 'docker build -t $IMAGE_NAME:$IMAGE_TAG .'
      }
    }

    stage('Push image (CI)') {
      steps {
        sh 'docker push $IMAGE_NAME:$IMAGE_TAG'
      }
    }

    stage('Deploy (CD)') {
      when {
        branch 'main'
      }
      steps {
        sh '''
          docker rm -f jenkins-demo-app || true
          docker run -d \
            --name jenkins-demo-app \
            -p 5000:5000 \
            $IMAGE_NAME:$IMAGE_TAG
        '''
      }
    }
  }
}
