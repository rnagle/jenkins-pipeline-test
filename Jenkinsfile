node {
  currentBuild.result = 'SUCCESS'

  try {
    stage('Cleanup') {
      // Delete the workspace before we begin
      step([$class: 'WsCleanup'])
    }

    stage('Checkout') {
      checkout scm
    }

    stage('Build') {
      echo 'Build process goes here!'
    }
  }
  catch(err) {
    currentBuild.result = 'FAILURE'
    throw err
  }
  finally {
    if (currentBuild.result == 'SUCCESS') {
      if (["FAILURE", "UNSTABLE"].contains(currentBuild.previousBuild.result)) {
        echo 'A (naive) indication that the build is stable again.'
      }
    }
  }
}
