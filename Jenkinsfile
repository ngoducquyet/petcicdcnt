node('master') {
  stage('Pull code') {
    checkout scm
  }
  stage('Unit test'){
    sh 'echo $BRANCH_NAME'
    sh 'echo ${BRANCH_NAME}'
    echo "${BRANCH_NAME}"
    echo BRANCH_NAME
    /echo env.BRANCH_NAME
  }
  /*
  stage('Run feature branch') {
      when {
          branch 'feature23' 
      }
      steps {
        sh 'echo ${BRANCH_NAME}'
      }
  }
  stage('Run develop branch') {
      when {
          branch 'develop'  
      }
      steps {
      sh 'echo develop branch'
      }
  }
}

*/