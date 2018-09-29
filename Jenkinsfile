pipeline {
  agent any
  stages {
    stage('Execute Code quality') {
      steps {
        sh '  sh "/var/lib/jenkins/apache-maven-3.5.4/bin/mvn sonar:sonar -Dsonar.host.url=http://sonarqube-jenkins.apps.na39.openshift.opentlc.com"'
      }
    }
    stage('Build and Execute unit test cases') {
      steps {
        sh '  sh "/var/lib/jenkins/apache-maven-3.5.4/bin/mvn clean package jacoco:prepare-agent jacoco:report -Dorg.jenkinsci.plugins.durabletask.BourneShellScript.HEARTBEAT_CHECK_INTERVAL=300"'
      }
    }
    stage('publish test cases result') {
      steps {
        junit '**/target/surefire-reports/TEST*.xml'
      }
    }
    stage('Record Jacoco coverage report') {
      steps {
        jacoco()
      }
    }
    stage('Build and Deploy Services to Dev environment') {
      parallel {
        stage('Build and Deploy Services to Dev environment') {
          steps {
            sh ''' sh "oc start-build testapp --from-file=target/spring-petclinic-openshift-2.0.0.BUILD-SNAPSHOT.jar --follow --loglevel 10"
      sh "sleep 90"'''
          }
        }
        stage('frontend micro-service') {
          steps {
            sh 'sh "sleep 90"'
          }
        }
        stage('admin micro-service') {
          steps {
            sh 'sh "sleep 90"'
          }
        }
      }
    }
    stage('Execute smoke tests') {
      steps {
        sh ' sh \'curl -I http://testapp-jenkins.apps.na39.openshift.opentlc.com/\''
      }
    }
    stage('Build and Deploy Services to QA environment') {
      steps {
        input 'Proceed to QA env ?'
      }
    }
    stage('Functional Testing') {
      steps {
        sh ' sh \'python FunctTest.py\''
      }
    }
    stage('Performance testing ') {
      steps {
        sh 'sh "sleep 90"'
      }
    }
  }
}