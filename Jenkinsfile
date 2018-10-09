node{
   stage('SCM Checkout'){
     git 'https://github.com/ngoducquyet/spring-framework-petclinic.git'
   }
   stage('Compile-Package'){
      // Get maven home path
      def mvnHome =  tool name: 'M3', type: 'maven'   
      sh "${mvnHome}/bin/mvn clean package -P MySQL"
   }
   stage('Deploy to Tomcat'){
      sh 'sudo cp target/*.war /opt/tomcat9/webapps'
      sh 'sudo rm -rf /opt/tomcat9/webapps/petclinic'
   }
   stage('Email Notification'){
      mail bcc: '', body: '''Hi Job petclinic is completed
      Thanks
      Quyet''', cc: '', from: '', replyTo: '', subject: 'Jenkins Job', to: 'ngoducquyet2018@gmail.com'
   }
   stage('Slack Notification'){
       slackSend baseUrl: 'https://ngoducquyet.slack.com/services/hooks/jenkins-ci/',
       channel: '#build',
       color: 'good', 
       message: 'Job petclinic is completed , Slack!'
   }
}
