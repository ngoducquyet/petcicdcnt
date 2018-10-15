pipeline {
  agent {
      label 'master'
  }
  stages {
    stage('Pull code') {
      checkout scm
    }
    stage('Test branch name') {
      sh 'echo $BRANCH_NAME'
      sh 'echo ${BRANCH_NAME}'
      echo "${BRANCH_NAME}"
      echo BRANCH_NAME
      echo env.BRANCH_NAME
    }
    stage('feature23') {
      when {
          branch 'feature23' 
      }
      steps {
        echo 'run feature'
      }
    }
    stage('Develop branch') {
      when {
          branch 'develop'  
      }
      steps {
        echo 'run develop'
      }
    }
  }
}
