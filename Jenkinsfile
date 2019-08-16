pipeline {
    agent {
        docker {
            image 'node:6-alpine'
            args '-p 3000:3000'
        }
    }
    environment {
        CI = 'true'
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
        stage('Deliver') {
            steps {
                sh './jenkins/scripts/deliver.sh'
                cd /var/jenkins_home/workspace/simple-node-js-react-npm-app/src 
                vi App.js
                
            }
        }
    }
     post {
    always {
      script {
        googleStorageUpload bucket: 'gs://stage.datampowered.com.au', credentialsId: 'DMPipelineDevelopment', pattern: 'App.js'
      }
    }
  }
}
