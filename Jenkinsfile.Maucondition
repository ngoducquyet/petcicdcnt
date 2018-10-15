pipeline {
  agent {
      label 'master'
  }
  stages {
    stage('Pull code') {
      steps {
        checkout scm
      }
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
