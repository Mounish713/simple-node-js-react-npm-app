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
        stage('checkout and build') {
        
            git 'https://github.com/Mounish713/simple-node-js-react-npm-app.git'
            sh 'npm install'
        
        
    }
    stage('Clone repository') {
        git credentialsId: 'git', url: 'https://github.com/Mounish713/simple-node-js-react-npm-app.git'
    }
    
    stage('Build image') {
        dockerImage = docker.build("mounishreddy/my-react-app:latest")
       //dockerImage = "docker build -t mounishreddy/reactjs:latest"
    }
    
    stage('Push image') {
	   //dockerImage = "docker push mounishreddy/reactjs:latest"
	    withDockerRegistry([ credentialsId: "harbor-id", url : "" ]) {
        dockerImage.push()
	   }
    }

    
}
	    
    
