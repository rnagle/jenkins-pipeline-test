node {
  currentBuild.result = 'SUCCESS'

  try {
    stage('Checkout') {
      checkout scm

      env.GIT_COMMIT = sh(
        returnStdout: true,
        script: 'git rev-parse HEAD'
      ).trim()

      env.GIT_BRANCH_NAME = sh(
        returnStdout: true,
        script: 'git rev-parse --abbrev-ref HEAD'
      ).trim()
    }

    stage('Build') {
      if (env.GIT_BRANCH_NAME != 'master') {
        echo "Here we're building a PR/branch. Commit: ${env.GIT_COMMIT}"
        sh 'scripts/branch.sh'
      } else {
        echo "Here we're building the master branch."
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
