<<<<<<< HEAD
def projectName = 'portfolio'
=======
def projectName = 'portfoliomanagement'
>>>>>>> 020864ad33b1e1d26d719c56be7cff08f908302f
def version = "0.0.${currentBuild.number}"
def dockerImageTag = "${projectName}:${version}"

pipeline {
  agent any

  stages {
     stage('Build docker image') {
          // this stage also builds and tests the Java project using Maven
          steps {
            sh "docker build -t ${dockerImageTag} ."
          }
      }
    stage('Deploy Container To Openshift') {
      environment {
           OPENSHIFT_CREDS = credentials('openshiftCreds')
           MYSQL_CREDS = credentials('MySQLCreds')
          }
      steps {
        sh "oc login -u ${OPENSHIFT_CREDS_USR} -p ${OPENSHIFT_CREDS_PSW}"
        sh "oc project ${projectName} || oc new-project ${projectName}"
        sh "oc delete all --selector app=${projectName} || echo 'Unable to delete all previous openshift resources'"
        sh "oc new-app ${dockerImageTag} -l version=${version}"
        sh "oc expose svc/${projectName}"
      }
    }
  }
}

