#!groovy

  //Getting the  env  global varibale values
 
  echo "GitHub BranhName ${env. BRANCH_NAME}"
  echo "Jenkins Job Number ${env.BUILD_NUMBER}"
  echo "Jenkins Node Name ${env.NODE_NAME}"
  
  echo "Jenkins Home ${env.JENKINS_HOME}"
  echo "Jenkins URL ${env.JENKINS_URL}"
  echo "JOB Name ${env.JOB_NAME}"
  
properties([
    buildDiscarder(logRotator(numToKeepStr: '3')),
    pipelineTriggers([
        pollSCM('* * * * *')
    ])
])
//properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '', numToKeepStr: '5'))])

//node('slave1') {
//node (label: 'slave1') {
//node('master'){


node {
  def mvnHome = tool name: 'M3_HOME', type: 'maven'  

  
   stage('Checkout the code'){
     git branch: 'master', credentialsId: 'xyz', url: 'https://github.com/d/hare-mvnproj.git'
  }
  
  stage('Build'){
     
    sh "${mvnHome}/bin/mvn clean package"
         
  }
   
 stage('SonarqubeReport'){
     
     sh "${mvnHome}/bin/mvn  sonar:sonar"
      
  }
  
  stage('Upload Artifacts into Nexus'){
     
     sh "${mvnHome}/bin/mvn clean deploy"
     
  }
  
  stage('DeployAppIntoTomcat'){
     
	  sh 'echo "Starting deployment"'
      sh 'cp $WORKSPACE/target/*.war /Users/Softwares/Runni/apache-tomcat-9.0.12/webapps/'
      sh 'echo "Deployment done successfully"'
	  
      
  }
 
  	stage('EmailNotification'){
	    mail to: 'dvv@gmail.com',
	         subject: "Job '${JOB_NAME}' # ${BUILD_NUMBER} is success",
	         body: "Please go to ${BUILD_URL} and verify the build \n"
  	   }
  	   
  	   
	  }
    
}
