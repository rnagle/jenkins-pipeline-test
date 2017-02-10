node {
  currentBuild.result = 'SUCCESS'

  try {
    stage('Debug') {
      echo "DEBUG: params"
      for ( e in params ) {
        println "key = ${e.key}, value = ${e.value}"
      }

      echo "DEBUG: branch and change id"
      echo "BRANCH_NAME: ${env.BRANCH_NAME}"
      echo "CHANGE_ID: ${env.CHANGE_ID}"
    }

    stage('Checkout') {
      checkout scm
    }

    stage('Build') {
      echo 'Build process goes here!'

      sh('ls -hl .')

      commit = sh(
        returnStdout: true,
        script: 'git rev-parse HEAD'
      ).trim()

      branch = sh(
        returnStdout: true,
        script: 'git rev-parse --abbrev-ref HEAD'
      ).trim()

      if (branch != 'master' && commit) {
        echo "Here we're building a PR/branch. Commit: ${commit}"
      } else {
        echo "Here we're building the master branch."
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
