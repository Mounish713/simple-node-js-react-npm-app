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
        stage('build and push docker image') {
          steps {
            container('nodejs') {
              sh 'docker build -t $REGISTRY/$DOCKERHUB_NAMESPACE/$APP_NAME:SNAPSHOT-$BRANCH_NAME-$BUILD_NUMBER .'
              withCredentials([usernamePassword(passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME', credentialsId: "$DOCKER_CREDENTIAL_ID",)]) {
                            sh 'echo "$DOCKER_PASSWORD" | docker login $REGISTRY -u "$DOCKER_USERNAME" --password-stdin'
                            sh 'docker push $REGISTRY/$DOCKERHUB_NAMESPACE/$APP_NAME:SNAPSHOT-$BRANCH_NAME-$BUILD_NUMBER'
        }
      }
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
               
    
