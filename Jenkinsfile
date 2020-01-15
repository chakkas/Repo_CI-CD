pipeline {
  agent {
    docker {
      image 'hashmapinc/sqitch:jenkins'
      args 'docker image for sqitch database change management'
    }

  }
  stages {
    stage('Stage1') {
      steps {
        build(job: 'job1', quietPeriod: 2, wait: true, propagate: true)
      }
    }

  }
}