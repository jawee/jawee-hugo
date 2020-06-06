---
title: "Jenkins Pipeline Hugo"
date: 2020-06-06T19:34:49+02:00
draft: false
---

Example Jenkinsfile for two stage deploy of a hugo website.

Triggers on a webhook from github, which triggers the build.
1. Checking out repository from git, including theme submodule.
2. Builds for beta-website, which is a simple test environment. 
3. Deploys to the beta-website.
4. Waits for input from user to continue to production.
5. Builds for production.
6. Deploys to production.

```groovy
properties([pipelineTriggers([githubPush()])])

pipeline {
  agent any
  
  stages {
    /* checkout repo */
    stage('Checkout SCM') {
      steps {
        checkout([
          $class: 'GitSCM',
          branches: [[name: 'master']],
          extensions: [[$class: 'SubmoduleOption',
                        disableSubmodules: false,
                        parentCredentials: false,
                        recursiveSubmodules: true,
                        reference: '',
                        trackingSubmodules: false]],
          userRemoteConfigs: [[
            url: 'https://github.com/user/repo-url.git',
            credentialsId: '',
          ]]
        ])
      }
    }
    stage('Build Beta') {
      steps {
        echo ">> Build application"
        sh "hugo -b https://beta.base.url"
      }
    }
    stage('Deploy Beta') {
      steps {
        sshagent(["linode"]) {
          sh 'rsync -r -e "ssh -o StrictHostKeyChecking=no" "$WORKSPACE/public/" user@host.tld:/path/to/site/public_html'
        }
      }
    }
    stage('Test before deploying live') {
      steps {
        input message: 'Do you want to release this build?',
              parameters: [[$class: 'BooleanParameterDefinition',
                            defaultValue: false,
                            description: 'Ticking this box will deploy to hellracers.se',
                            name: 'Release']]
      }
    }
    stage('Build Live') {
      steps {
        echo ">> Build application"
        sh "hugo -b https://base.url"
      }
    }
    stage('Deploy Live') {
      steps {
        sshagent(["linode"]) {
          sh 'rsync -r -e "ssh -o StrictHostKeyChecking=no" "$WORKSPACE/public/" user@host.tld:/path/to/site/public_html'
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
