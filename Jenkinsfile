@Library('dst-shared@master') _

pipeline {

  agent { node { label 'dstbuild' } }

  environment {
    PRODUCT = 'csm'
    RELEASE_TAG = setReleaseTag()
  }

  stages {

    stage('Package') {
      steps {
        packageHelmCharts(chartsPath: "${env.WORKSPACE}/kubernetes",
                          buildResultsPath: "${env.WORKSPACE}/build/results",
                          buildDate: "${env.BUILD_DATE}")
      }
    }

    stage('Publish') {
      steps {
        publishHelmCharts(chartsPath: "${env.WORKSPACE}/kubernetes")
      }
    }

    stage('Push to github') {
        when { allOf {
            expression { BRANCH_NAME ==~ /(release\/.*|master)/ }
        }}
        steps {
            script {
                pushToGithub(
                    githubRepo: "Cray-HPE/cray-nexus",
                    pemSecretId: "githubapp-stash-sync",
                    githubAppId: "91129",
                    githubAppInstallationId: "13313749"
                )
            }
        }
    }

  }

  post {
    success {
      findAndTransferArtifacts()
    }
  }

}
