pipeline {
  agent any
  stages {
    stage('code quality') {
      steps {
        sh 'sleep "90"'
      }
    }
    stage('Build and Execute unit test cases') {
      steps {
        sh 'sleep "90"'
      }
    }
    stage('test result') {
      steps {
        junit '**/target/surefire-reports/TEST*.xml'
      }
    }
    stage('jacoco') {
      steps {
        jacoco()
      }
    }
    stage('deploy to dev env') {
      parallel {
        stage('deploy to dev env') {
          steps {
            sh 'sleep "90"'
          }
        }
        stage('admin micro-service') {
          steps {
            sh 'sleep "90"'
          }
        }
        stage('config micro service') {
          steps {
            sh 'sleep "90"'
          }
        }
      }
    }
    stage('smoke test ') {
      steps {
        sh 'sleep "90"'
      }
    }
    stage('deploy to QA env.') {
      steps {
        input 'proceed to QA ?'
      }
    }
    stage('functional tests') {
      steps {
        sh 'sleep "90"'
      }
    }
    stage('performance tests ') {
      steps {
        sh 'sleep "90"'
      }
    }
  }
}