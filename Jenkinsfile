properties([pipelineTriggers([githubPush()])])

pipeline {
  agent any
  
  stages {
    stage('Build') {
      steps {
        echo ">> Build application"
        sh "hugo -b https://jawee.se"
      }
    }
    stage('Deploy') {
      steps {
        sshagent(["linode"]) {
          sh 'rsync -r -e "ssh -o StrictHostKeyChecking=no" "$WORKSPACE/public/" figge@jawee.se:/home/figge/public/jawee.se/public_html'
        }
      }
    }
  }
  /* Cleanup workspace */
    post {
       always {
           deleteDir()
       }
   }
} 
