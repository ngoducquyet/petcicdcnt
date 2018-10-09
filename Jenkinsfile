//node('docker') {
node{
  stage('Pull') {
      checkout scm
//     git 'https://github.com/ngoducquyet/petcicd.git'
  }
//  stage('Compile-Package'){
 //     //def mvnHome =  tool name: 'M3', type: 'maven'   
      //sh "${mvnHome}/bin/mvn clean package -P MySQL"
 //    sh "mvn clean package -P MySQL"
 // }
  stage('Build & Unit test'){
      sh 'mvn clean -P MySQL verify -DskipITs=true';
      junit '**/target/surefire-reports/TEST-*.xml'
      archive 'target/*.jar'
 }
  stage('Static Code Analysis'){
//    sh 'mvn clean verify sonar:sonar -Dsonar.host.url=http://jenkins-master:9000 -Dsonar.projectName=example-project -Dsonar.projectKey=example-project -Dsonar.projectVersion=$BUILD_NUMBER';
      sh 'mvn clean verify sonar:sonar -Dsonar.host.url=http://jenkins-master:9000 -Dsonar.projectName=pet-project -Dsonar.projectKey=pet-project -Dsonar.projectVersion=$BUILD_NUMBER';

  }
// stage ('Integration Test'){
//    sh 'mvn clean verify -Dsurefire.skip=true';
 //   junit '**/target/failsafe-reports/TEST-*.xml'
  //  archive 'target/*.jar'
  //} 
  stage ('Publish'){
    def server = Artifactory.server 'Default Artifactory Server'
    def uploadSpec = """{
      "files": [
        {
          "pattern": "target/petclinic.war",
          "target": "pet-project-ci/${BUILD_NUMBER}/",
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
  stage ('Start Tomcat'){
    sh '''cd /home/jenkins/tomcat/bin
    ./startup.sh''';
  }
  stage ('Deploy '){
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
        sh 'curl -u${credentials} -X PUT "http://jenkins-master:8081/artifactory/api/storage/pet-project-cd/ ${BUILD_NUMBER}/petclinic.war?properties=Performance-Tested=Yes"';
      }
  }
}
