properties([
  parameters([
    string(defaultValue: '', description: '', name: 'stable_tag')
  ])
])

node {
  currentBuild.result = 'SUCCESS'

  try {
    stage('Checkout') {
      checkout scm

      env.GIT_COMMIT = sh(
        returnStdout: true,
        script: 'git rev-parse HEAD'
      ).trim()
    }

    if (params.stable_tag) {
      echo "This is a release to production."
      sh "STABLE_TAG=${params.stable_tag} scripts/release.sh"
    } else if (env.BRANCH_NAME != 'master') {
      echo "Here we're building a PR/branch. Commit: ${env.GIT_COMMIT}"
      wrap([$class: 'Xvfb']) {
        withEnv(["PATH=${tool 'NodeJS 4.2.6'}/bin:${env.PATH}"]) {
          sh 'scripts/branch.sh'
        }
      }
    } else {
      echo "Here we're building the master/base branch."
      withEnv(["PATH=${tool 'NodeJS 4.2.6'}/bin:${env.PATH}"]) {
        sh 'scripts/master.sh'
      }
    }
  }
  catch(err) {
    currentBuild.result = 'FAILURE'
    throw err
  }
  finally {
    if (currentBuild.result == 'SUCCESS' && currentBuild.previousBuild) {
      if (["FAILURE", "UNSTABLE"].contains(currentBuild.previousBuild.result)) {
        echo 'A (naive) indication that the build is stable again.'
      }
    }
  }
}
