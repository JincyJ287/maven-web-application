node{
def mavenHome = tool name: 'maven3.9.8'
properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), pipelineTriggers([cron('* * * * *')])])
echo "Job name is ${env.JOB_NAME}"
echo "Build Number is ${env.BUILD_NUMBER}"
echo "The node name is ${env.NODE_NAME}"
echo "The job URL is ${env.JOB_URL}"
stage('checkoutCode'){
git branch: 'development', credentialsId: '73960293-f0f1-4cad-8c4f-58aa52fc324e', url: 'https://github.com/JincyJ287/maven-web-application.git'
}

stage('Build'){
sh "$mavenHome/bin/mvn clean package"
}

stage('ExecuteSonarQubeReport'){
sh "$mavenHome/bin/mvn clean sonar:sonar"
}

stage('UploadArtifactsIntoNexus'){
sh "$mavenHome/bin/mvn clean deploy"
}

stage('DeployAppToTomcat'){
sshagent(['e1afb78c-a98b-43b4-9d21-838db12b3c27']) {
sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@172.31.5.59:/opt/apache-tomcat-9.0.96/webapps"
}
}
    
}

