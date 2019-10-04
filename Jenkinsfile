#!/usr/bin/env groovy
properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '3', artifactNumToKeepStr: '8', daysToKeepStr: '3', numToKeepStr: '8')), disableConcurrentBuilds() ])

node {
  try {
    stage('Checkout') {
      def branch = env.BRANCH_NAME
      checkout([
        $class: 'GitSCM', 
        branches: [[name: "*/" + branch]],
        doGenerateSubmoduleConfigurations: false, 
        extensions: [[$class: 'CloneOption', depth: 1, noTags: false, reference: '', shallow: true]], 
        submoduleCfg: [], 
        userRemoteConfigs: [[
          url: 'git@github.com:ankursingh081/fastlane-example.git'
        ]]
      ])
    }

}finally {
    stage('Clean Workspace') {
      cleanWs()
    }
  }
 }