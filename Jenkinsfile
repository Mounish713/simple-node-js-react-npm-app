pipeline {
    agent {
     node {
      label 'nodejs'
     }
    }
    environment {
     DOCKER_CREDENTIAL_ID = 'harbor-id'
     REGISTRY = '172.30.102.24:30002'
     DOCKERHUB_NAMESPACE = 'library'
     APP_NAME = 'nextg-ui'
     NODEJS_HOME = "${tool 'nodejs-16.13.1'}"
     PATH="${env.NODEJS_HOME}/bin:${env.PATH}"
    }

     
    
    
    stages {
        stage('Build') {
            steps {
                sh 'npm install'
            }
        }
        stage('Test') {
                    steps {
                        sh './jenkins/scripts/test.sh'
                    }
                }
       
  stage("Remove the local Docker image") {
      steps {
        container('nodejs') {
          sh '''
            docker image rm $REGISTRY/$DOCKERHUB_NAMESPACE/$APP_NAME:SNAPSHOT-$BRANCH_NAME-$BUILD_NUMBER
          '''
        }
      }
    }
  }  
}
               
    
