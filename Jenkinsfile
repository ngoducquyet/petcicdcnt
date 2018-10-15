node('master') {
  stage('Pull code') {
    checkout scm
  }
  stage('Unit test'){
    sh 'echo $BRANCH_NAME'
    sh 'echo ${BRANCH_NAME}'
    echo "${BRANCH_NAME}"
    echo BRANCH_NAME
    echo env.BRANCH_NAME
  }
