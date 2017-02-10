node {
  currentBuild.result = 'SUCCESS'

  try {
    stage('Debug') {
      echo "DEBUG: params"
      for ( e in params ) {
        println "key = ${e.key}, value = ${e.value}"
      }

      echo "DEBUG: env"
      for ( e in env ) {
        println "key = ${e.key}, value = ${e.value}"
      }
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
    if (currentBuild.result == 'SUCCESS' && currentBuild.previousBuild) {
      if (["FAILURE", "UNSTABLE"].contains(currentBuild.previousBuild.result)) {
        echo 'A (naive) indication that the build is stable again.'
      }
    }
  }
}
