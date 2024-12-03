node {
def mavenHome = tool name : 'maven3.9.5'

properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'RebuildSettings', autoRebuild: false, rebuildDisabled: false], pipelineTriggers([pollSCM('* * * * *')])])

stage ('Checkout') {
    git branch: 'development', url: 'https://github.com/shivashankardr/maven-web-application.git'
}
stage ('Build'){
    sh "${mavenHome}/bin/mvn clean package"
}
stage ('ExecuteSonarqubeReport') {
    sh "${mavenHome}/bin/mvn clean sonar:sonar"
}
stage ('UploadArtefactsIntoNexus') {
    sh "${mavenHome}/bin/mvn clean deploy"
}
stage ('DeployAppIntoTomcat') {
    sshagent(['083150f1-575b-4651-9ea0-c3ba873edfaa']) {
   sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@172.31.10.130:/opt/apache-tomcat-9.0.97/webapps/"
   
}
}
}
