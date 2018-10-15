 
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

  stage('Run feature branch') {
      def branch = $BRANCH_NAME
      when {
          branch 'feature23' 
      }
      steps {
        echo 'run feature roi'
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
