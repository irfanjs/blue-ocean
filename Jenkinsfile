pipeline {
  agent any
  stages {
    stage('code quality') {
      steps {
        sh ' sh "/var/lib/jenkins/apache-maven-3.5.4/bin/mvn sonar:sonar -Dsonar.host.url=http://sonarqube-jenkins.apps.na39.openshift.opentlc.com"'
      }
    }
    stage('Build and Execute unit test cases') {
      steps {
        sh ' sh "/var/lib/jenkins/apache-maven-3.5.4/bin/mvn clean package jacoco:prepare-agent jacoco:report -Dorg.jenkinsci.plugins.durabletask.BourneShellScript.HEARTBEAT_CHECK_INTERVAL=300"'
      }
    }
    stage('Test cases result') {
      steps {
        junit '**/target/surefire-reports/TEST*.xml'
      }
    }
    stage('Jacoco') {
      steps {
        jacoco()
      }
    }
    stage('deploy') {
      parallel {
        stage('deploy') {
          steps {
            sh ' sh "oc start-build testapp --from-file=target/spring-petclinic-openshift-2.0.0.BUILD-SNAPSHOT.jar --follow --loglevel 10"'
          }
        }
        stage('frontend micro service ') {
          steps {
            sh ' sh "oc start-build testapp --from-file=target/spring-petclinic-openshift-2.0.0.BUILD-SNAPSHOT.jar --follow --loglevel 10"'
          }
        }
        stage('config service ') {
          steps {
            sh ' sh "oc start-build testapp --from-file=target/spring-petclinic-openshift-2.0.0.BUILD-SNAPSHOT.jar --follow --loglevel 10"'
          }
        }
      }
    }
    stage('smoke tests') {
      steps {
        sh ' sh \'curl -I http://testapp-jenkins.apps.na39.openshift.opentlc.com/\''
      }
    }
    stage('deploy to QA') {
      steps {
        input '\'Proceed to QA environment ?\''
      }
    }
    stage('functional tests') {
      steps {
        sh ' sh \'python FunctTest.py\''
      }
    }
    stage('performance tests') {
      steps {
        sh ' sh \'/var/lib/jenkins/apache-maven-3.5.4/bin/mvn verify\''
      }
    }
  }
}