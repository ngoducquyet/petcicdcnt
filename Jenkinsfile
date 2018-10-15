 
node('master') {
  stage('Pull code') {
    checkout scm
  }
  def branch = ${BRANCH_NAME}
  stage('Unit test'){
    sh 'echo $BRANCH_NAME'
    sh 'echo ${BRANCH_NAME}'
    echo "${BRANCH_NAME}"
    echo BRANCH_NAME
    echo env.BRANCH_NAME
  }
  stage('Run feature branch') {
      when {
          branch 'feature23' 
      }
      steps {
        echo "${BRANCH_NAME}"
      }
  }
  stage('Run develop branch') {
      when {
          branch 'develop'  
      }
      steps {
      echo 'develop branch'
      }
  }
}
