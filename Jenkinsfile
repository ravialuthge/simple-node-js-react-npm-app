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
                input message: 'Finished using the web site? (Click "Proceed" to continue)'
                sh './jenkins/scripts/kill.sh'
                sh ' cat /var/jenkins_home/workspace/simple-node-js-react-npm-app/src/App.js '
                sh ' mkdir abc '
                sh ' cp -r /var/jenkins_home/workspace/simple-node-js-react-npm-app/build abc '
            }
        }
    }
     post {
    always {
      script {
        googleStorageUpload bucket: 'gs://stage.datampowered.com.au', credentialsId: 'DMPipelineDevelopment', pattern: 'abc' , showInline: true
      }
    }
  }
}
