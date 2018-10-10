
node{
  stage('Pull code') {
      checkout scm
  }
  stage('Unit test'){
      sh 'mvn clean -P MySQL verify -DskipITs=true';
      junit '**/target/surefire-reports/TEST-*.xml'
      archive 'target/*.jar'
  }

  stage('Static Code Analysis'){
      sh 'mvn clean -P MySQL verify sonar:sonar -Dsonar.host.url=http://jenkins-master:9000 -Dsonar.projectName=pet-project -Dsonar.projectKey=pet-project -Dsonar.projectVersion=$BUILD_NUMBER';
  }

  stage ('Publish to Artifactory'){
    def server = Artifactory.server 'Default Artifactory Server'
    def uploadSpec = """{
      "files": [
        {
          "pattern": "target/petclinic.war",
          "target": "pet-project-cd/${BUILD_NUMBER}/",
          "props": "Integration-Tested=Yes;Performance-Tested=No"
        }
      ]
    }"""
    server.upload(uploadSpec)
  }
  
  stash includes: 'target/petclinic.war,src/test/jmeter/petclinic_test_plan.jmx',
  name: 'binary'
}
node('docker_pt') {
  stage ('QA Auto test Environment'){
    sh '''cd /home/jenkins/tomcat/bin
    ./startup.sh''';
    unstash 'binary'
    sh 'cp target/petclinic.war /home/jenkins/tomcat/webapps/';
  }
  stage ('Performance Testing'){
    sh '''cd /opt/jmeter/bin/
    ./jmeter.sh -n -t $WORKSPACE/src/test/jmeter/petclinic_test_plan.jmx -l $WORKSPACE/test_report.jtl''';
    step([$class: 'ArtifactArchiver', artifacts: '**/*.jtl'])
  }
  stage ('Promote build in Artifactory'){
    withCredentials([usernameColonPassword(credentialsId:
      'artifactory-account', variable: 'credentials')]) {
        sh 'curl -u${credentials} -X PUT "http://jenkins-master:8081/artifactory/api/storage/pet-project-cd/${BUILD_NUMBER}/petclinic.war?properties=Performance-Tested=Yes"';
      }
  }
}

node {
  stage ('Deploy Staging enviroment'){
    unstash 'binary'
    sh 'cp target/petclinic.war /opt/tomcat/webapps/petclinic${BUILD_NUMBER}.war';
  }

  stage('Email Notification'){
      mail bcc: '', body: '''Hi there, job petclinic${BUILD_NUMBER} is completed
      Thanks
      Quyet''', cc: '', from: '', replyTo: '', subject: 'Jenkins Deploy Job', to: 'ngoducquyet2018@gmail.com'
  }
  stage('Slack Notification'){
      slackSend baseUrl: 'https://ngoducquyet.slack.com/services/hooks/jenkins-ci/',
      channel: '#build',
      color: 'good', 
      message: 'Job petclinic${BUILD_NUMBER} env.BRANCH_NAME is completed, Slack!'
  }
  stage('Test'){
    sh 'echo $BRANCH_NAME'
    sh 'echo "${BRANCH_NAME}"'
    sh 'echo BRANCH_NAME'
    sh 'echo env.BRANCH_NAME'
  }
}
