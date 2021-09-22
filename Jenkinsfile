node
{
//properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')),pipelineTriggers([pollSCM('* * * * *')])])

properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), pipelineTriggers([githubPush()])])
def mavenHome = tool name: "maven 3.8.2"

stage('checkOutCode')
{
git branch: 'dev', credentialsId: '790d9d01-86d2-48df-b2a9-72f18105394b',
 url: 'https://github.com/ramyaravindra-ec-apps/maven-web-application.git'
}

/* sh "mvn package" (for linux)-----> if jenkins and maven in same linux machine
 bat "mvn package" (for windows)
 but maven is installed in jenkins (3.8.2) 
 so path of maven in jenkins is /var/lib/jenkins/tools/hudson.tasks.Maven_MavenInstallation/maven_3.8.2
 and mvn command is found in bin directory of maven */

stage('Build')
{
sh "${mavenHome}/bin/mvn clean package" 
}

stage('ExecuteSonarQubeReport')
{
sh "${mavenHome}/bin/mvn clean package sonar:sonar"
}

stage('Upload Artifact To Nexus')
{
sh "${mavenHome}/bin/mvn deploy"
}

//ssh agent plugin for tomcat bcoz tomcat and jenkins are in different servers

stage('DeployToTomcat')
{

//if permission denied error give read permision to webapps---> dir chmod -R 777 webapps/

sshagent(['7f075cae-dccc-4910-9139-94e291580778'])
 {
    sh "scp  -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@54.152.205.58:/opt/apache-tomcat-9.0.53/webapps/"

}
}
/*
stage('sendEmailNotification')
{

mail bcc: '', body: '''buildover

Regards,
ramya.''', cc: '', from: '', replyTo: '', subject: 'buildover', to: 'ramyarocks541@gmail.com'
}
*/

}
