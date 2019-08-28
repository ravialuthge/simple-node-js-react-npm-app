pipeline {
    agent {
        docker {
            image 'node:6-alpine'
            args '-p 3000:3000'
        }
    }
    environment {
        CI = 'true'
        PATTERN = 'asset-manifest.json , favicon.ico , index.html , manifest.json , service-worker.js , static/'
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
            }
        }
        stage('Store to GCS') {
            steps{
                sh 'cp -r /var/jenkins_home/workspace/simple-node-js-react-npm-app/build/. /var/jenkins_home/workspace/simple-node-js-react-npm-app'
            }
        }
    }
     post {
      always {
       script {
        googleStorageUpload bucket: 'gs://test_datampowered', credentialsId: 'DMPipelineDevelopment', pattern: env.PATTERN , showInline: true
           sh ' rm -rf env.PATTERN '
      }
    }
         failure {
            mail to: 'mailstesting6@gmail.com',
                subject: "Failed Pipeline: ${currentBuild.fullDisplayName}",
                body: "Something is wrong with ${env.BUILD_URL}"
         }
  }
}
