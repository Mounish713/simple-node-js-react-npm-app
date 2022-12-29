pipeline {
    agent {
     node {
      label 'nodejs'
     }
    }
    environment {
     DOCKER_CREDENTIAL_ID = 'harbor-id'
     REGISTRY = '172.30.102.24:30002'
     HARBOR_NAMESPACE = 'library'
     APP_NAME = 'Demo-React-App'
     NODEJS_HOME = "${tool 'nodejs-16.13.1'}"
     PATH="${env.NODEJS_HOME}/bin:${env.PATH}"
    }
    stages {
	stage('Install') {
            steps {
              container('nodejs') {
                sh 'npm install'
              }
            }
        }

         stage('build') {
            steps {
              container('nodejs') {
                sh 'npm run build'
              }
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
                  sh 'docker build -t $REGISTRY/$HARBOR_NAMESPACE/$APP_NAME:SNAPSHOT-$BRANCH_NAME-$BUILD_NUMBER .'
                  withCredentials([usernamePassword(passwordVariable: 'HARBOR_PASSWORD', usernameVariable: 'HARBOR_USERNAME', credentialsId: "$HARBOR_CREDENTIAL_ID",)]) {
                                sh 'echo "$HARBOR_PASSWORD" | docker login $REGISTRY -u "$HARBOR_USERNAME" --password-stdin'
                                sh 'docker push $REGISTRY/$HARBORHUB_NAMESPACE/$APP_NAME:SNAPSHOT-$BRANCH_NAME-$BUILD_NUMBER'
        }
      }
    }
  }

     
    }
    
    
}
	    
    
