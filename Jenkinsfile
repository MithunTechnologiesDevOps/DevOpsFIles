node
{
 
 properties([[$class: 'JiraProjectProperty'], buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), pipelineTriggers([pollSCM('* * * * *')])])
 
 echo "Build number is:  ${env.BUILD_NUMBER}"
 
 def mavenHome = tool name: "maven3.6.3"
 
 stage('CheckoutCode')
 {
  git branch: 'development', credentialsId: '91ad3536-8f9f-4983-85d9-daf832f30b35', url: 'https://github.com/MithunTechnologiesDevOps/maven-web-application.git'
 }

 stage('Build')
 {
  sh "${mavenHome}/bin/mvn clean package"
 }
 
 stage('ExecuteSonarQubeReport')
  {
  sh "${mavenHome}/bin/mvn sonar:sonar"
  }
  
  stage('UploadArtifactIntoNexus')
  {
  sh "${mavenHome}/bin/mvn deploy"
  }
  
  stage('DeployAppInto TomcatServer')
  {
  sshagent(['TomcatServerCredentential']) 
  {
   sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war  ec2-user@3.128.95.103:/opt/tomcat9/webapps/"
  }
  
  }
  
  stage('EmailNotification')
  {
  mail bcc: 'polepallip@gmail.com', body: '''Build is Over..

  Regards,
  Mithun Technologies,
  9980923226''', cc: 'areddy121314@gmail.com', from: '', replyTo: '', subject: 'Build is over..', to: 'devopstrainingblr@gmail.com,anuroop.rv@gmail.com'
  }

}
