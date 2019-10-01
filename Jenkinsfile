#!/usr/bin/env groovy
import io.rcl.labs.jenkins.libraries.IosFastlanePipeline
properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '3', artifactNumToKeepStr: '8', daysToKeepStr: '3', numToKeepStr: '8')), disableConcurrentBuilds() ])

node {

  def hash = env.BUILD_TAG.hashCode().toString().replaceAll('-', '')
  ws("ws/${hash}") {
    try {
        checkoutStage('ssh://git@bitbucket.rccl.com:7999/~ankursingh_rccl.com/excalibur-ios.git', branch)

    	if (env.BRANCH_NAME == 'fastlane-build-deploy') {

                env.FASTLANE_XCODEBUILD_SETTINGS_TIMEOUT = 120
                env.PIPELINE_YML_DIR = "${env.WORKSPACE}/ios"

                def buildAnd = IosFastlanePipeline.Builder(this)
                    .checkout([dir: "ios",
                      branches: [[name: "*/${env.BRANCH_NAME}"]],
                      extensions: [[$class: 'CloneOption', honorRefspec: true, noTags: true, reference: '', shallow: true, timeout: 30]],
                      userRemoteConfigs:[[credentialsId: 'leeroy-jenkins-ssh', url: 'git@github.com:ankursingh081/fastlane-example.git']]])
                    .context([develop: [target: "Royal-Dev", schema: "Royal-Dev", workspace: "${env.WORKSPACE}/ios/excalibur.xcworkspace", project: "${env.WORKSPACE}/ios/excalibur.xcodeproj"]
                             ])
                    .appCenter([filePath: "${env.WORKSPACE}/Royal-Dev.ipa"])
                    .build()

                buildAnd.execute()

        }
    } catch(caughtError) {
        currentBuild.result = "FAILURE"
        throw caughtError
    } finally {
        deleteDir()
    }
  }
}
def checkoutStage(repo, branch) {
    stage('Checkout'){
        ansiMessage("Checkout", "BLUE")
        if(branch.startsWith('PR-')) {
            def prNumber = branch.split('-')[1]
            branch = "develop . && git fetch --depth 1 origin refs/pull-requests/${prNumber}/from:pr-test && git checkout pr-test"
        }else {
            branch = branch + ' .'
        }
        sshagent (credentials: ['ankursingh081']) {
            sh "git clone ${repo} --depth 1 -b ${branch}"
            sh "ls -Al"
        }
    }
}
